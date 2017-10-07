---
title: "Kurz: Azure Active Directory integrace s Menlo zabezpečení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Menlo zabezpečení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 193d12eedf31f4f08e1d141936d6e918c36a2109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="2faf4-103">Kurz: Azure Active Directory integrace s Menlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2faf4-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="2faf4-104">V tomto kurzu zjistíte, jak toointegrate Menlo zabezpečení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2faf4-104">In this tutorial, you learn how toointegrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2faf4-105">Integrace Menlo zabezpečení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2faf4-105">Integrating Menlo Security with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2faf4-106">Můžete řídit ve službě Azure AD, který má přístup tooMenlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2faf4-106">You can control in Azure AD who has access tooMenlo Security</span></span>
- <span data-ttu-id="2faf4-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMenlo zabezpečení (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2faf4-107">You can enable your users tooautomatically get signed-on tooMenlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2faf4-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2faf4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2faf4-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="2faf4-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="2faf4-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2faf4-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2faf4-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2faf4-111">Prerequisites</span></span>

<span data-ttu-id="2faf4-112">tooconfigure integrace Azure AD s Menlo zabezpečení, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2faf4-112">tooconfigure Azure AD integration with Menlo Security, you need hello following items:</span></span>

- <span data-ttu-id="2faf4-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2faf4-113">An Azure AD subscription</span></span>
- <span data-ttu-id="2faf4-114">Zabezpečení Menlo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2faf4-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2faf4-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2faf4-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2faf4-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2faf4-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2faf4-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2faf4-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2faf4-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2faf4-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2faf4-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2faf4-119">Scenario description</span></span>
<span data-ttu-id="2faf4-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2faf4-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2faf4-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2faf4-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2faf4-122">Přidání Menlo zabezpečení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2faf4-122">Adding Menlo Security from hello gallery</span></span>
2. <span data-ttu-id="2faf4-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2faf4-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-hello-gallery"></a><span data-ttu-id="2faf4-124">Přidání Menlo zabezpečení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2faf4-124">Adding Menlo Security from hello gallery</span></span>
<span data-ttu-id="2faf4-125">tooconfigure hello integraci Menlo zabezpečení do služby Azure AD, je nutné tooadd Menlo zabezpečení hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2faf4-125">tooconfigure hello integration of Menlo Security into Azure AD, you need tooadd Menlo Security from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2faf4-126">**tooadd Menlo zabezpečení z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2faf4-126">**tooadd Menlo Security from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2faf4-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2faf4-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2faf4-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2faf4-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2faf4-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2faf4-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2faf4-134">Hello vyhledávacího pole zadejte **Menlo zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-134">In hello search box, type **Menlo Security**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="2faf4-136">Na panelu výsledků hello vyberte **Menlo zabezpečení**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2faf4-136">In hello results panel, select **Menlo Security**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2faf4-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2faf4-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2faf4-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Menlo zabezpečení založené na testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2faf4-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2faf4-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Menlo zabezpečení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2faf4-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Menlo Security is tooa user in Azure AD.</span></span> <span data-ttu-id="2faf4-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Menlo zabezpečení musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2faf4-141">In other words, a link relationship between an Azure AD user and hello related user in Menlo Security needs toobe established.</span></span>

<span data-ttu-id="2faf4-142">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2faf4-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Menlo Security.</span></span>

<span data-ttu-id="2faf4-143">tooconfigure a testu Azure AD jednotné přihlašování s Menlo zabezpečení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2faf4-143">tooconfigure and test Azure AD single sign-on with Menlo Security, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2faf4-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2faf4-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2faf4-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2faf4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2faf4-146">**[Vytvoření zkušebního uživatele Menlo zabezpečení](#creating-a-menlo-security-test-user)**  -toohave protějšek Britta Simon v Menlo zabezpečení, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2faf4-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - toohave a counterpart of Britta Simon in Menlo Security that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2faf4-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2faf4-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2faf4-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2faf4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2faf4-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2faf4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2faf4-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Menlo zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="2faf4-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="2faf4-151">**tooconfigure Azure AD jednotné přihlašování s Menlo zabezpečení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2faf4-151">**tooconfigure Azure AD single sign-on with Menlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="2faf4-152">V portálu Azure, na hello hello **Menlo zabezpečení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-152">In hello Azure portal, on hello **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2faf4-154">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2faf4-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="2faf4-156">Na hello **Menlo zabezpečení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2faf4-156">On hello **Menlo Security Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="2faf4-158">a.</span><span class="sxs-lookup"><span data-stu-id="2faf4-158">a.</span></span> <span data-ttu-id="2faf4-159">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="2faf4-159">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="2faf4-160">b.</span><span class="sxs-lookup"><span data-stu-id="2faf4-160">b.</span></span> <span data-ttu-id="2faf4-161">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="2faf4-161">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2faf4-162">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="2faf4-162">These values are not hello real.</span></span> <span data-ttu-id="2faf4-163">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2faf4-163">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2faf4-164">Obraťte se na [tým podpory zabezpečení klienta Menlo](https://www.menlosecurity.com/menlo-contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2faf4-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="2faf4-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2faf4-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="2faf4-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2faf4-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2faf4-169">Na hello **konfigurace zabezpečení Menlo** klikněte na tlačítko **konfigurace zabezpečení Menlo** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2faf4-169">On hello **Menlo Security Configuration** section, click **Configure Menlo Security** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2faf4-170">Kopírování hello **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2faf4-170">Copy hello **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="2faf4-172">tooconfigure jednotného přihlašování na **Menlo zabezpečení** straně, přihlášení toohello **Menlo zabezpečení** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="2faf4-172">tooconfigure single sign-on on **Menlo Security** side, login toohello **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="2faf4-173">V části **nastavení** přejděte příliš**ověřování** a proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="2faf4-173">Under **Settings** go too**Authentication** and perform following actions:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="2faf4-175">a.</span><span class="sxs-lookup"><span data-stu-id="2faf4-175">a.</span></span> <span data-ttu-id="2faf4-176">Zaškrtněte políčko hello **povolení ověřování uživatele pomocí SAML**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-176">Tick hello checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="2faf4-177">b.</span><span class="sxs-lookup"><span data-stu-id="2faf4-177">b.</span></span> <span data-ttu-id="2faf4-178">Vyberte **povolit externí přístup** příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-178">Select **Allow External Access** too**Yes**.</span></span>

    <span data-ttu-id="2faf4-179">c.</span><span class="sxs-lookup"><span data-stu-id="2faf4-179">c.</span></span> <span data-ttu-id="2faf4-180">V části **SAML zprostředkovatele**, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="2faf4-181">d.</span><span class="sxs-lookup"><span data-stu-id="2faf4-181">d.</span></span> <span data-ttu-id="2faf4-182">**Koncový bod SAML 2.0** : vložení hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2faf4-182">**SAML 2.0 Endpoint** : Paste hello **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2faf4-183">e.</span><span class="sxs-lookup"><span data-stu-id="2faf4-183">e.</span></span> <span data-ttu-id="2faf4-184">**Identifikátor služby (Vystavitel)** : vložení hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2faf4-184">**Service Identifier (Issuer)** : Paste hello **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2faf4-185">f.</span><span class="sxs-lookup"><span data-stu-id="2faf4-185">f.</span></span> <span data-ttu-id="2faf4-186">**Certifikát X.509** : Otevřete hello **certifikátu (Base64)** stáhli z portálu Azure hello v programu Poznámkový blok a vložte ho v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="2faf4-186">**X.509 Certificate** : Open hello **Certificate (Base64)** downloaded from hello Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="2faf4-187">g.</span><span class="sxs-lookup"><span data-stu-id="2faf4-187">g.</span></span> <span data-ttu-id="2faf4-188">Klikněte na tlačítko **Uložit** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="2faf4-188">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="2faf4-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2faf4-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2faf4-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2faf4-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2faf4-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2faf4-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2faf4-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2faf4-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="2faf4-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2faf4-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2faf4-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2faf4-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2faf4-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2faf4-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2faf4-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2faf4-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2faf4-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2faf4-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2faf4-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2faf4-204">a.</span><span class="sxs-lookup"><span data-stu-id="2faf4-204">a.</span></span> <span data-ttu-id="2faf4-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2faf4-206">b.</span><span class="sxs-lookup"><span data-stu-id="2faf4-206">b.</span></span> <span data-ttu-id="2faf4-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2faf4-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2faf4-208">c.</span><span class="sxs-lookup"><span data-stu-id="2faf4-208">c.</span></span> <span data-ttu-id="2faf4-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2faf4-210">d.</span><span class="sxs-lookup"><span data-stu-id="2faf4-210">d.</span></span> <span data-ttu-id="2faf4-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="2faf4-212">Vytvoření zkušebního uživatele Menlo zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2faf4-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="2faf4-213">V této části vytvoříte volal Britta Simon v Menlo zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="2faf4-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="2faf4-214">Práce s [tým podpory zabezpečení klienta Menlo](https://www.menlosecurity.com/menlo-contact) tooadd hello uživatelé v platformě Menlo zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="2faf4-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) tooadd hello users in hello Menlo Security platform.</span></span> <span data-ttu-id="2faf4-215">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2faf4-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2faf4-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2faf4-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2faf4-217">V této části povolíte tak, že udělíte přístup tooMenlo zabezpečení Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2faf4-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMenlo Security.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2faf4-219">**tooassign Britta Simon tooMenlo zabezpečení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2faf4-219">**tooassign Britta Simon tooMenlo Security, perform hello following steps:**</span></span>

1. <span data-ttu-id="2faf4-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2faf4-222">V seznamu aplikace hello vyberte **Menlo zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-222">In hello applications list, select **Menlo Security**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="2faf4-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2faf4-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2faf4-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2faf4-226">Click **Add** button.</span></span> <span data-ttu-id="2faf4-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2faf4-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2faf4-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2faf4-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2faf4-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2faf4-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2faf4-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2faf4-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2faf4-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2faf4-232">Testing single sign-on</span></span>

<span data-ttu-id="2faf4-233">V této části můžete vyzkoušet Azure AD jeden přihlašování v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2faf4-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="2faf4-234">Otevřete okno prohlížeče v "InPrivate" nebo "Incognito" režimu tootrigger nové ověřování.</span><span class="sxs-lookup"><span data-stu-id="2faf4-234">Open a browser window in an "InPrivate" or "Incognito" mode tootrigger a new authentication.</span></span>  <span data-ttu-id="2faf4-235">V aplikaci Internet Explorer použijte kombinaci kláves Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="2faf4-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="2faf4-236">V prohlížeči Chrome použijte kombinaci kláves Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="2faf4-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="2faf4-237">V okně privátní procházení hello procházet tooa chráněný prostředek a provést přihlášení Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2faf4-237">In hello private browsing window, browse tooa protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="2faf4-238">Po úspěšném přihlášení bude požadovaný webu přijatá toohello v relaci izolace.</span><span class="sxs-lookup"><span data-stu-id="2faf4-238">Upon successful login, you will be taken toohello requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2faf4-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2faf4-239">Additional resources</span></span>

* [<span data-ttu-id="2faf4-240">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2faf4-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2faf4-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2faf4-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

