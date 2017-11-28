---
title: "Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Symantec webové zabezpečení služby (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="90f6c-103">Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="90f6c-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="90f6c-104">V tomto kurzu se dozvíte, jak toointegrate vaší společnosti Symantec webové zabezpečení služby (WSS) účet pomocí účtu Azure Active Directory (Azure AD), aby se WSS koncový uživatel zřízené v hello Azure AD ověřit pomocí ověřování SAML a vynutit uživatele nebo pravidla úrovni zásady skupiny.</span><span class="sxs-lookup"><span data-stu-id="90f6c-104">In this tutorial, you will learn how toointegrate your Symantec Web Security Service (WSS) account with your Azure Active Directory (Azure AD) account so that WSS can authenticate an end user provisioned in hello Azure AD using SAML authentication and enforce user or group level policy rules.</span></span>

<span data-ttu-id="90f6c-105">Integrace Symantec webové zabezpečení služby (WSS) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="90f6c-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="90f6c-106">Spravujte všechny hello koncoví uživatelé a skupiny používané účtu WSS z portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f6c-106">Manage all of hello end users and groups used by your WSS account from your Azure AD portal.</span></span> 

- <span data-ttu-id="90f6c-107">Povolí hello koncoví uživatelé tooauthenticate sami ve WSS pomocí svých přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f6c-107">Allow hello end users tooauthenticate themselves in WSS using their Azure AD credentials.</span></span>

- <span data-ttu-id="90f6c-108">Povolte hello vynucení uživatele a skupiny zásadou na úrovni pravidla definovaná v účtu WSS.</span><span class="sxs-lookup"><span data-stu-id="90f6c-108">Enable hello enforcement of user and group level policy rules defined in your WSS account.</span></span>

<span data-ttu-id="90f6c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90f6c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90f6c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90f6c-110">Prerequisites</span></span>

<span data-ttu-id="90f6c-111">tooconfigure integrace Azure AD s Symantec webové zabezpečení služby (WSS), je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="90f6c-111">tooconfigure Azure AD integration with Symantec Web Security Service (WSS), you need hello following items:</span></span>

- <span data-ttu-id="90f6c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f6c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="90f6c-113">Účet služby pro zabezpečení webové Symantec (WSS)</span><span class="sxs-lookup"><span data-stu-id="90f6c-113">A Symantec Web Security Service (WSS) account</span></span>

> [!NOTE]
> <span data-ttu-id="90f6c-114">tootest hello kroky v tomto kurzu, nedoporučujeme pomocí WSS účtu, který je aktuálně používá pro produkční účely.</span><span class="sxs-lookup"><span data-stu-id="90f6c-114">tootest hello steps in this tutorial, we do not recommend using a WSS account that is currently being used for production purpose.</span></span>

<span data-ttu-id="90f6c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="90f6c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="90f6c-116">Nepoužívejte účtu WSS, který je aktuálně používá pro produkční účely pro tento test Pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="90f6c-116">Do not use your WSS account that is currently being used for production purpose for this test unless it is necessary.</span></span>
- <span data-ttu-id="90f6c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90f6c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="90f6c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="90f6c-118">Scenario description</span></span>
<span data-ttu-id="90f6c-119">V tomto kurzu nakonfigurujete vaší služby Azure AD tooenable jeden přihlašování tooWSS pomocí přihlašovacích údajů koncového uživatele hello definované v účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f6c-119">In this tutorial, you will configure your Azure AD tooenable single sign-on tooWSS using hello end user credentials defined in your Azure AD account.</span></span>
<span data-ttu-id="90f6c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="90f6c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90f6c-121">Přidání aplikace hello Symantec webové zabezpečení služby (WSS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="90f6c-121">Adding hello Symantec Web Security Service (WSS) app from hello gallery</span></span>
2. <span data-ttu-id="90f6c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f6c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a><span data-ttu-id="90f6c-123">Přidání Symantec webové zabezpečení služby (WSS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="90f6c-123">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
<span data-ttu-id="90f6c-124">tooconfigure hello integrace Symantec webové zabezpečení služby (WSS) do Azure AD, je nutné tooadd Symantec webové zabezpečení služby (WSS) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="90f6c-124">tooconfigure hello integration of Symantec Web Security Service (WSS) into Azure AD, you need tooadd Symantec Web Security Service (WSS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="90f6c-125">**tooadd Symantec webové zabezpečení služby (WSS) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90f6c-125">**tooadd Symantec Web Security Service (WSS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="90f6c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="90f6c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="90f6c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="90f6c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="90f6c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90f6c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="90f6c-133">Hello vyhledávacího pole zadejte **Symantec webové zabezpečení služby (WSS)**, vyberte **Symantec webové zabezpečení služby (WSS)** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="90f6c-133">In hello search box, type **Symantec Web Security Service (WSS)**, select **Symantec Web Security Service (WSS)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Symantec webové zabezpečení služby (WSS) v seznamu výsledků hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="90f6c-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f6c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="90f6c-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="90f6c-136">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="90f6c-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Symantec webové zabezpečení služby (WSS) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90f6c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Symantec Web Security Service (WSS) is tooa user in Azure AD.</span></span> <span data-ttu-id="90f6c-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Symantec webové zabezpečení služby (WSS) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="90f6c-138">In other words, a link relationship between an Azure AD user and hello related user in Symantec Web Security Service (WSS) needs toobe established.</span></span>

<span data-ttu-id="90f6c-139">V Symantec webové zabezpečení služby (WSS) přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="90f6c-139">In Symantec Web Security Service (WSS), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="90f6c-140">tooconfigure a testování Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="90f6c-140">tooconfigure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="90f6c-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="90f6c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="90f6c-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90f6c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90f6c-143">**[Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  -toohave protějšek Britta Simon v zabezpečení webové Symantec služby (WSS), je toohello propojené služby Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="90f6c-143">**[Create a Symantec Web Security Service (WSS) test user](#create-a-symantec-web-security-service-wss-test-user)** - toohave a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="90f6c-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90f6c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90f6c-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="90f6c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="90f6c-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f6c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="90f6c-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="90f6c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="90f6c-148">**tooconfigure Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90f6c-148">**tooconfigure Azure AD single sign-on with Symantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="90f6c-149">V portálu Azure, na hello hello **Symantec webové zabezpečení služby (WSS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-149">In hello Azure portal, on hello **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="90f6c-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90f6c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="90f6c-153">Na hello **Symantec webové zabezpečení služby (WSS) domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="90f6c-153">On hello **Symantec Web Security Service (WSS) Domain and URLs** section, perform hello following steps:</span></span>

    ![Symantec webové zabezpečení služby (WSS) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="90f6c-155">a.</span><span class="sxs-lookup"><span data-stu-id="90f6c-155">a.</span></span> <span data-ttu-id="90f6c-156">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="90f6c-156">In hello **Identifier** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    <span data-ttu-id="90f6c-157">b.</span><span class="sxs-lookup"><span data-stu-id="90f6c-157">b.</span></span> <span data-ttu-id="90f6c-158">V hello **adresa URL odpovědi** textovému poli, hello zadat adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span><span class="sxs-lookup"><span data-stu-id="90f6c-158">In hello **Reply URL** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span></span>

    > [!NOTE]
    > <span data-ttu-id="90f6c-159">Obraťte se na hello [tým podpory Symantec webové zabezpečení služby (WSS) klienta](https://www.symantec.com/contact-us) Pokud hello hodnoty pro hello **identifikátor** a **adresa URL odpovědi** nefungují z nějakého důvodu.</span><span class="sxs-lookup"><span data-stu-id="90f6c-159">Please contact hello [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) if hello values for hello **Identifier** and **Reply URL** are not working for some reason.</span></span>

4. <span data-ttu-id="90f6c-160">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="90f6c-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="90f6c-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90f6c-162">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="90f6c-164">tooconfigure jednotného přihlašování na hello straně Symantec webové zabezpečení služby (WSS), naleznete v online dokumentaci WSS toohello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-164">tooconfigure single sign-on on hello Symantec Web Security Service (WSS) side, refer toohello WSS online documentation.</span></span> <span data-ttu-id="90f6c-165">Hello Stáhnout **soubor XML s metadaty** soubor bude nutné importovat do portálu WSS hello toobe.</span><span class="sxs-lookup"><span data-stu-id="90f6c-165">hello downloaded **Metadata XML** file will need toobe imported into hello WSS portal.</span></span> <span data-ttu-id="90f6c-166">Kontaktujte hello [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us) Pokud potřebujete pomoc s konfigurací hello na portálu WSS hello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-166">Contact hello [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) if you need assistance with hello configuration on hello WSS portal.</span></span>

> [!TIP]
> <span data-ttu-id="90f6c-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="90f6c-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="90f6c-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="90f6c-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="90f6c-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="90f6c-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="90f6c-170">Create an Azure AD test user</span></span>

<span data-ttu-id="90f6c-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="90f6c-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90f6c-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="90f6c-174">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90f6c-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="90f6c-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="90f6c-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90f6c-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="90f6c-180">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="90f6c-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    <span data-ttu-id="90f6c-182">a.</span><span class="sxs-lookup"><span data-stu-id="90f6c-182">a.</span></span> <span data-ttu-id="90f6c-183">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="90f6c-184">b.</span><span class="sxs-lookup"><span data-stu-id="90f6c-184">b.</span></span> <span data-ttu-id="90f6c-185">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90f6c-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="90f6c-186">c.</span><span class="sxs-lookup"><span data-stu-id="90f6c-186">c.</span></span> <span data-ttu-id="90f6c-187">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="90f6c-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="90f6c-188">d.</span><span class="sxs-lookup"><span data-stu-id="90f6c-188">d.</span></span> <span data-ttu-id="90f6c-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-189">Click **Create**.</span></span>
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="90f6c-190">Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="90f6c-190">Create a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="90f6c-191">V této části vytvoříte uživatele volat Britta Simon v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="90f6c-191">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="90f6c-192">Hello odpovídající end uživatelské jméno je možné ručně vytvořit hello WSS portálu nebo můžete počkat, hello uživatele nebo skupiny zřízené v portálu WSS hello Azure AD synchronizovány toobe toohello po několika minutách (~ 15 minut).</span><span class="sxs-lookup"><span data-stu-id="90f6c-192">hello corresponding end username can be manually created in hello WSS portal or you can wait for hello users/groups provisioned in hello Azure AD toobe synchronized toohello WSS portal after a few minutes (~15 minutes).</span></span> <span data-ttu-id="90f6c-193">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90f6c-193">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="90f6c-194">Hello veřejnou IP adresu počítače hello koncového uživatele, který bude použité toobrowse weby také potřebovat toobe zřízené v portálu společnosti Symantec webové zabezpečení služby (WSS) hello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-194">hello public IP address of hello end user machine, which will be used toobrowse websites also need toobe provisioned in hello Symantec Web Security Service (WSS) portal.</span></span>

> [!NOTE]
> <span data-ttu-id="90f6c-195">Prosím [kliknutím sem](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget váš počítač je veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="90f6c-195">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget your machine's public IPaddress.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="90f6c-196">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="90f6c-196">Assign hello Azure AD test user</span></span>

<span data-ttu-id="90f6c-197">V této části povolíte tak, že udělíte přístup tooSymantec zabezpečení webové služby (WSS) toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="90f6c-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSymantec Web Security Service (WSS).</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="90f6c-199">**tooassign tooSymantec Britta Simon webové zabezpečení služby (WSS), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="90f6c-199">**tooassign Britta Simon tooSymantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="90f6c-200">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="90f6c-202">V seznamu aplikace hello vyberte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-202">In hello applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![odkaz Hello Symantec webové zabezpečení služby (WSS) v seznamu aplikace hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. <span data-ttu-id="90f6c-204">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="90f6c-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="90f6c-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="90f6c-206">Click **Add** button.</span></span> <span data-ttu-id="90f6c-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90f6c-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="90f6c-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="90f6c-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="90f6c-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90f6c-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="90f6c-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="90f6c-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="90f6c-212">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="90f6c-212">Test single sign-on</span></span>

<span data-ttu-id="90f6c-213">V této části budete testovat hello jeden přihlašování funkce nyní, když jste nakonfigurovali vaší toouse WSS účet služby Azure AD pro ověřování SAML.</span><span class="sxs-lookup"><span data-stu-id="90f6c-213">In this section, you'll test hello single sign-on functionality now that you've configured your WSS account toouse your Azure AD for SAML authentication.</span></span>

<span data-ttu-id="90f6c-214">Po konfiguraci vaší webové prohlížeče tooproxy provoz tooWSS, když jste otevřete ve webovém prohlížeči a zkuste to toobrowse tooa lokality, pak budete stránku přesměrovaného toohello Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="90f6c-214">After you have configured your web browser tooproxy traffic tooWSS, when you open your web browser and try toobrowse tooa site then you'll be redirected toohello Azure sign-on page.</span></span> <span data-ttu-id="90f6c-215">Zadejte přihlašovací údaje hello hello testovací koncového uživatele, která byla zajištěna v hello Azure AD (BrittaSimon) a přidružené heslo.</span><span class="sxs-lookup"><span data-stu-id="90f6c-215">Enter hello credentials of hello test end user that has been provisioned in hello Azure AD (that is, BrittaSimon) and associated password.</span></span> <span data-ttu-id="90f6c-216">Po ověření, budete moct toobrowse toohello web, který jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="90f6c-216">Once authenticated, you'll be able toobrowse toohello website that you chose.</span></span> <span data-ttu-id="90f6c-217">Měli jste vytvoření pravidla zásad na hello WSS straně tooblock BrittaSimon z procházení tooa určité lokalitě, potom hello WSS bloku stránky měli vidět, pokusíte-li jako uživatel BrittaSimon toobrowse toothat lokality.</span><span class="sxs-lookup"><span data-stu-id="90f6c-217">Should you create a policy rule on hello WSS side tooblock BrittaSimon from browsing tooa particular site then you should see hello WSS block page when you attempt toobrowse toothat site as user BrittaSimon.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90f6c-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="90f6c-218">Additional resources</span></span>

* [<span data-ttu-id="90f6c-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="90f6c-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90f6c-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="90f6c-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

