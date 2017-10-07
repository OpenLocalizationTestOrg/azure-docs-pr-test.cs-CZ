---
title: "Kurz: Integrace Azure Active Directory této společnosti Enterprise Vault.cloud jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování Vault.cloud Enterprise této společnosti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1037e70515686091460ac41e9e5a7951639bb520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a><span data-ttu-id="63f68-103">Kurz: Integrace Azure Active Directory pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-103">Tutorial: Azure Active Directory integration with Veritas Enterprise Vault.cloud SSO</span></span>

<span data-ttu-id="63f68-104">V tomto kurzu zjistíte, jak toointegrate této společnosti Enterprise Vault.cloud jednotné přihlašování s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="63f68-104">In this tutorial, you learn how toointegrate Veritas Enterprise Vault.cloud SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="63f68-105">Integrace této společnosti Enterprise Vault.cloud jednotné přihlašování s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="63f68-105">Integrating Veritas Enterprise Vault.cloud SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="63f68-106">Můžete řídit ve službě Azure AD, který má přístup tooVeritas Enterprise Vault.cloud SSO</span><span class="sxs-lookup"><span data-stu-id="63f68-106">You can control in Azure AD who has access tooVeritas Enterprise Vault.cloud SSO</span></span>
- <span data-ttu-id="63f68-107">Vaši uživatelé tooautomatically get přihlášeného tooVeritas Enterprise Vault.cloud SSO (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="63f68-107">You can enable your users tooautomatically get signed-on tooVeritas Enterprise Vault.cloud SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="63f68-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="63f68-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="63f68-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="63f68-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63f68-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="63f68-110">Prerequisites</span></span>

<span data-ttu-id="63f68-111">tooconfigure integrace Azure AD pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, potřebujete hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="63f68-111">tooconfigure Azure AD integration with Veritas Enterprise Vault.cloud SSO, you need hello following items:</span></span>

- <span data-ttu-id="63f68-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="63f68-112">An Azure AD subscription</span></span>
- <span data-ttu-id="63f68-113">Této společnosti Enterprise Vault.cloud jednotného přihlašování k jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="63f68-113">A Veritas Enterprise Vault.cloud SSO single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="63f68-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="63f68-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="63f68-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="63f68-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="63f68-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="63f68-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="63f68-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="63f68-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="63f68-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="63f68-118">Scenario description</span></span>
<span data-ttu-id="63f68-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="63f68-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="63f68-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="63f68-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="63f68-121">Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="63f68-121">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
2. <span data-ttu-id="63f68-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-hello-gallery"></a><span data-ttu-id="63f68-123">Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="63f68-123">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
<span data-ttu-id="63f68-124">tooconfigure hello integrace této společnosti Enterprise Vault.cloud přihlašování do služby Azure AD, je nutné tooadd této společnosti Enterprise Vault.cloud SSO hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="63f68-124">tooconfigure hello integration of Veritas Enterprise Vault.cloud SSO into Azure AD, you need tooadd Veritas Enterprise Vault.cloud SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="63f68-125">**tooadd této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63f68-125">**tooadd Veritas Enterprise Vault.cloud SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="63f68-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="63f68-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="63f68-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="63f68-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="63f68-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="63f68-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="63f68-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63f68-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="63f68-133">Hello vyhledávacího pole zadejte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="63f68-133">In hello search box, type **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. <span data-ttu-id="63f68-135">Na panelu výsledků hello vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="63f68-135">In hello results panel, select **Veritas Enterprise Vault.cloud SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="63f68-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="63f68-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="63f68-138">In this section, you configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="63f68-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v této společnosti Enterprise Vault.cloud jednotné přihlašování je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="63f68-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veritas Enterprise Vault.cloud SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="63f68-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v této společnosti Enterprise Vault.cloud jednotné přihlašování musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="63f68-140">In other words, a link relationship between an Azure AD user and hello related user in Veritas Enterprise Vault.cloud SSO needs toobe established.</span></span>

<span data-ttu-id="63f68-141">V této společnosti Enterprise Vault.cloud jednotné přihlašování, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="63f68-141">In Veritas Enterprise Vault.cloud SSO, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="63f68-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="63f68-142">tooconfigure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="63f68-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="63f68-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="63f68-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="63f68-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="63f68-145">**[Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování k](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  -toohave protějšek Britta Simon SSO Vault.cloud Enterprise této společnosti, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="63f68-145">**[Creating a Veritas Enterprise Vault.cloud SSO test user](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)** - toohave a counterpart of Britta Simon in Veritas Enterprise Vault.cloud SSO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="63f68-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63f68-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="63f68-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="63f68-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="63f68-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="63f68-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v této společnosti Enterprise Vault.cloud jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="63f68-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veritas Enterprise Vault.cloud SSO application.</span></span>

<span data-ttu-id="63f68-150">**tooconfigure Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63f68-150">**tooconfigure Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="63f68-151">V portálu Azure, na hello hello **této společnosti Enterprise Vault.cloud jednotného přihlašování k** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="63f68-151">In hello Azure portal, on hello **Veritas Enterprise Vault.cloud SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="63f68-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63f68-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. <span data-ttu-id="63f68-155">Na hello **této společnosti Enterprise Vault.cloud jednotného přihlašování k doméně a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="63f68-155">On hello **Veritas Enterprise Vault.cloud SSO Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    <span data-ttu-id="63f68-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span><span class="sxs-lookup"><span data-stu-id="63f68-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="63f68-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="63f68-158">This value is not real.</span></span> <span data-ttu-id="63f68-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63f68-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="63f68-160">Obraťte se na [tým podpory této společnosti Enterprise Vault.cloud jednotné přihlašování klienta](https://www.veritas.com/support/.html) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="63f68-160">Contact [Veritas Enterprise Vault.cloud SSO Client support team](https://www.veritas.com/support/.html) tooget this value.</span></span> 

4. <span data-ttu-id="63f68-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="63f68-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. <span data-ttu-id="63f68-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63f68-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="63f68-165">Na hello **konfiguraci této společnosti Enterprise Vault.cloud jednotného přihlašování k** klikněte na tlačítko **konfigurace této společnosti Enterprise Vault.cloud SSO** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="63f68-165">On hello **Veritas Enterprise Vault.cloud SSO Configuration** section, click **Configure Veritas Enterprise Vault.cloud SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="63f68-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="63f68-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. <span data-ttu-id="63f68-168">tooconfigure jednotného přihlašování na **této společnosti Enterprise Vault.cloud jednotného přihlašování k** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby**příliš[tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html).</span><span class="sxs-lookup"><span data-stu-id="63f68-168">tooconfigure single sign-on on **Veritas Enterprise Vault.cloud SSO** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html).</span></span>

> [!TIP]
> <span data-ttu-id="63f68-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="63f68-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="63f68-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="63f68-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="63f68-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="63f68-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="63f68-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="63f68-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="63f68-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="63f68-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="63f68-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63f68-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="63f68-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="63f68-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="63f68-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="63f68-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="63f68-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="63f68-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="63f68-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="63f68-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="63f68-184">a.</span><span class="sxs-lookup"><span data-stu-id="63f68-184">a.</span></span> <span data-ttu-id="63f68-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="63f68-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="63f68-186">b.</span><span class="sxs-lookup"><span data-stu-id="63f68-186">b.</span></span> <span data-ttu-id="63f68-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="63f68-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="63f68-188">c.</span><span class="sxs-lookup"><span data-stu-id="63f68-188">c.</span></span> <span data-ttu-id="63f68-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="63f68-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="63f68-190">d.</span><span class="sxs-lookup"><span data-stu-id="63f68-190">d.</span></span> <span data-ttu-id="63f68-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="63f68-191">Click **Create**.</span></span>
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a><span data-ttu-id="63f68-192">Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-192">Creating a Veritas Enterprise Vault.cloud SSO test user</span></span>

<span data-ttu-id="63f68-193">V této části vytvoříte uživatele volat Britta Simon SSO Vault.cloud Enterprise.</span><span class="sxs-lookup"><span data-stu-id="63f68-193">In this section, you create a user called Britta Simon in Enterprise Vault.cloud SSO.</span></span> <span data-ttu-id="63f68-194">Práce s [tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html) pro přidání uživatelů hello hello Enterprise Vault.cloud jednotného přihlašování k platformě.</span><span class="sxs-lookup"><span data-stu-id="63f68-194">Work with [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html) to add hello users in hello Enterprise Vault.cloud SSO platform.</span></span> <span data-ttu-id="63f68-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63f68-195">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="63f68-196">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="63f68-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="63f68-197">V této části povolíte tak, že udělíte přístup tooVeritas Enterprise Vault.cloud jednotného přihlašování k toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="63f68-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeritas Enterprise Vault.cloud SSO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="63f68-199">**tooassign Britta Simon tooVeritas Enterprise Vault.cloud jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="63f68-199">**tooassign Britta Simon tooVeritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="63f68-200">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="63f68-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="63f68-202">V seznamu aplikace hello vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="63f68-202">In hello applications list, select **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. <span data-ttu-id="63f68-204">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="63f68-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="63f68-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="63f68-206">Click **Add** button.</span></span> <span data-ttu-id="63f68-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63f68-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="63f68-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="63f68-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="63f68-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63f68-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="63f68-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="63f68-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="63f68-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="63f68-212">Testing single sign-on</span></span>

<span data-ttu-id="63f68-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="63f68-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="63f68-214">Když kliknete na dlaždici hello této společnosti Enterprise Vault.cloud jednotné přihlašování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour této společnosti Enterprise Vault.cloud jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="63f68-214">When you click hello Veritas Enterprise Vault.cloud SSO tile in hello Access Panel, you should get automatically signed-on tooyour Veritas Enterprise Vault.cloud SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63f68-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="63f68-215">Additional resources</span></span>

* [<span data-ttu-id="63f68-216">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63f68-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="63f68-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="63f68-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

