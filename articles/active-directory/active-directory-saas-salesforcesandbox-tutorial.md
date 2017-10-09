---
title: "Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a izolovaného prostoru služby Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="ababb-103">Kurz: Azure Active Directory integrace s izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="ababb-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="ababb-104">V tomto kurzu zjistíte, jak toointegrate Salesforce izolovaného prostoru se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ababb-104">In this tutorial, you learn how toointegrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ababb-105">Integrace služby Salesforce izolovaného prostoru s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ababb-105">Integrating Salesforce Sandbox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ababb-106">Můžete řídit ve službě Azure AD, který má přístup tooSalesforce izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="ababb-106">You can control in Azure AD who has access tooSalesforce Sandbox</span></span>
- <span data-ttu-id="ababb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSalesforce izolovaného prostoru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ababb-107">You can enable your users tooautomatically get signed-on tooSalesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ababb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ababb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ababb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ababb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ababb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ababb-110">Prerequisites</span></span>

<span data-ttu-id="ababb-111">tooconfigure integrace Azure AD s Salesforce izolovaného prostoru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ababb-111">tooconfigure Azure AD integration with Salesforce Sandbox, you need hello following items:</span></span>

- <span data-ttu-id="ababb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ababb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ababb-113">Izolovaného prostoru Salesforce jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ababb-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ababb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ababb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ababb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ababb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ababb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ababb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ababb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ababb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ababb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ababb-118">Scenario description</span></span>
<span data-ttu-id="ababb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ababb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ababb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ababb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ababb-121">Přidání izolovaného prostoru Salesforce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ababb-121">Adding Salesforce Sandbox from hello gallery</span></span>
2. <span data-ttu-id="ababb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ababb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a><span data-ttu-id="ababb-123">Přidání izolovaného prostoru Salesforce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ababb-123">Adding Salesforce Sandbox from hello gallery</span></span>
<span data-ttu-id="ababb-124">tooconfigure hello integrace služby Salesforce izolovaného prostoru do Azure AD, je nutné tooadd izolovaného prostoru Salesforce hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ababb-124">tooconfigure hello integration of Salesforce Sandbox into Azure AD, you need tooadd Salesforce Sandbox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ababb-125">**tooadd izolovaného prostoru Salesforce z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ababb-125">**tooadd Salesforce Sandbox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ababb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ababb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ababb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ababb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ababb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ababb-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ababb-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ababb-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ababb-133">Hello vyhledávacího pole zadejte **izolovaného prostoru Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="ababb-133">In hello search box, type **Salesforce Sandbox**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="ababb-135">Na panelu výsledků hello vyberte **izolovaného prostoru Salesforce**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ababb-135">In hello results panel, select **Salesforce Sandbox**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ababb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ababb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ababb-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s izolovaného prostoru Salesforce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="ababb-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ababb-139">Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v izolovaném prostoru Salesforce je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ababb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Salesforce Sandbox is tooa user in Azure AD.</span></span> <span data-ttu-id="ababb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v izolovaném prostoru Salesforce musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ababb-140">In other words, a link relationship between an Azure AD user and hello related user in Salesforce Sandbox needs toobe established.</span></span>

<span data-ttu-id="ababb-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v izolovaném prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ababb-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="ababb-142">tooconfigure a testu Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ababb-142">tooconfigure and test Azure AD single sign-on with Salesforce Sandbox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ababb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ababb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ababb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ababb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ababb-145">**[Vytvoření zkušebního uživatele izolovaného prostoru Salesforce](#creating-a-salesforce-sandbox-test-user)**  -toohave protějšek Britta Simon v Salesforce izolovaného prostoru, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ababb-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - toohave a counterpart of Britta Simon in Salesforce Sandbox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ababb-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ababb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ababb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ababb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ababb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ababb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ababb-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="ababb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="ababb-150">**tooconfigure Azure AD jednotné přihlašování s Salesforce izolovaného prostoru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ababb-150">**tooconfigure Azure AD single sign-on with Salesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="ababb-151">V portálu Azure, na hello hello **izolovaného prostoru Salesforce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ababb-151">In hello Azure portal, on hello **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ababb-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ababb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="ababb-155">Na hello **Salesforce izolovaného prostoru domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ababb-155">On hello **Salesforce Sandbox Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="ababb-157">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="ababb-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ababb-158">Tato hodnota není skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-158">This value is not hello real.</span></span> <span data-ttu-id="ababb-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování. Obraťte se na [tým podpory služby Salesforce izolovaného prostoru klienta](https://help.salesforce.com/support) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ababb-159">Update this value with hello actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) tooget this value.</span></span>


4. <span data-ttu-id="ababb-160">Pokud jste již nakonfigurovali jednotné přihlašování pro jiná instance Salesforce izolovaného prostoru v adresáři, pak je také potřeba nakonfigurovat hello **identifikátor** toohave hello stejnou hodnotu jako hello **přihlásit na adrese URL**.</span><span class="sxs-lookup"><span data-stu-id="ababb-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="ababb-161">Hello **identifikátor** pole naleznete kontrolou hello **zobrazit upřesňující nastavení** zaškrtávací políčko je na hello **konfigurace adresy URL aplikace** stránku hello dialogového okna</span><span class="sxs-lookup"><span data-stu-id="ababb-161">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog</span></span> 


5. <span data-ttu-id="ababb-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikát** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ababb-162">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="ababb-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ababb-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="ababb-166">Na hello **Salesforce izolovaného prostoru konfigurace** klikněte na tlačítko **konfigurace izolovaného prostoru Salesforce** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="ababb-166">On hello **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ababb-167">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="ababb-167">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="ababb-168">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="ababb-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="ababb-169">V prohlížeči otevřete novou kartu a přihlaste se tooyour účet správce izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ababb-169">Open a new tab in your browser and log in tooyour Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="ababb-170">V nabídce hello hello nahoře, klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="ababb-170">In hello menu on hello top, click **Setup**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="ababb-172">V navigačním podokně hello na levé straně hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ababb-172">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="ababb-174">V hello část nastavení jednotného přihlašování, proveďte následující kroky hello: ![nakonfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="ababb-174">On hello Single Sign-On Settings section, perform hello following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="ababb-175">a.</span><span class="sxs-lookup"><span data-stu-id="ababb-175">a.</span></span>  <span data-ttu-id="ababb-176">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="ababb-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="ababb-177">b.</span><span class="sxs-lookup"><span data-stu-id="ababb-177">b.</span></span>  <span data-ttu-id="ababb-178">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="ababb-178">Click **New**.</span></span>

12. <span data-ttu-id="ababb-179">V hello oddílu SAML jeden přihlašování nastavení proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ababb-179">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="ababb-181">textového pole Název hello a.In, název typu hello hello konfigurace (např: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="ababb-181">a.In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="ababb-182">b.</span><span class="sxs-lookup"><span data-stu-id="ababb-182">b.</span></span> <span data-ttu-id="ababb-183">Vložení **SMAL Entity ID** hodnotu do hello **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ababb-183">Paste **SMAL Entity ID** value into hello **Issuer** textbox.</span></span>

    <span data-ttu-id="ababb-184">c.</span><span class="sxs-lookup"><span data-stu-id="ababb-184">c.</span></span> <span data-ttu-id="ababb-185">V hello **Entity Id** textovému poli, typ **https://test.salesforce.com** Pokud je první instance Salesforce izolovaného prostoru, které přidáváte tooyour directory hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-185">In hello **Entity Id** textbox, type **https://test.salesforce.com** if it is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="ababb-186">Pokud jste již přidali instance Salesforce karantény, pak pro hello **Entity ID** typu v hello **přihlašovací adresa URL**, což by mělo být v tomto formátu:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="ababb-186">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="ababb-187">d.</span><span class="sxs-lookup"><span data-stu-id="ababb-187">d.</span></span> <span data-ttu-id="ababb-188">Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát.</span><span class="sxs-lookup"><span data-stu-id="ababb-188">Click **Browse** tooupload hello downloaded certificate.</span></span>  

    <span data-ttu-id="ababb-189">e.</span><span class="sxs-lookup"><span data-stu-id="ababb-189">e.</span></span> <span data-ttu-id="ababb-190">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.</span><span class="sxs-lookup"><span data-stu-id="ababb-190">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
 
    <span data-ttu-id="ababb-191">f.</span><span class="sxs-lookup"><span data-stu-id="ababb-191">f.</span></span> <span data-ttu-id="ababb-192">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentifier hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="ababb-192">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="ababb-193">g.</span><span class="sxs-lookup"><span data-stu-id="ababb-193">g.</span></span> <span data-ttu-id="ababb-194">Vložení **jeden přihlašování adresa URL služby** do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ababb-194">Paste **Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="ababb-195">h.</span><span class="sxs-lookup"><span data-stu-id="ababb-195">h.</span></span> <span data-ttu-id="ababb-196">SFDC nepodporuje SAML odhlášení.</span><span class="sxs-lookup"><span data-stu-id="ababb-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="ababb-197">Jako alternativní řešení, vložte 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' do hello **adresa URL odhlašovací zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="ababb-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="ababb-198">i.</span><span class="sxs-lookup"><span data-stu-id="ababb-198">i.</span></span> <span data-ttu-id="ababb-199">Jako **zprostředkovatele iniciované žádosti vazby služby**, vyberte **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="ababb-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="ababb-200">j.</span><span class="sxs-lookup"><span data-stu-id="ababb-200">j.</span></span> <span data-ttu-id="ababb-201">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ababb-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="ababb-202">Povolit doménu</span><span class="sxs-lookup"><span data-stu-id="ababb-202">Enable your domain</span></span>
<span data-ttu-id="ababb-203">V této části se předpokládá, že jste již vytvořili domény.</span><span class="sxs-lookup"><span data-stu-id="ababb-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="ababb-204">Další informace najdete v tématu [definování váš název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="ababb-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="ababb-205">**tooenable doménu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ababb-205">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="ababb-206">V levém navigačním podokně hello, klikněte na **Správa domén**a pak klikněte na tlačítko **Moje domény.**</span><span class="sxs-lookup"><span data-stu-id="ababb-206">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="ababb-208">Přesvědčte se, že doménu je správně nakonfigurováno.</span><span class="sxs-lookup"><span data-stu-id="ababb-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="ababb-209">V hello **nastavení přihlašovací stránky** klikněte na tlačítko **upravit**, pak jako **ověřovací služby**, vyberte název hello hello SAML jeden přihlašování nastavení z předchozí hello část a nakonec klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ababb-209">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="ababb-211">Jakmile máte doménu nakonfigurován, uživatelé měli používat hello domény adresy URL toologin toohello Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="ababb-211">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="ababb-212">Hodnota hello tooget hello adresy URL, klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-212">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="ababb-213">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ababb-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ababb-214">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ababb-215">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ababb-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ababb-216">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ababb-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="ababb-217">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ababb-219">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ababb-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ababb-220">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ababb-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ababb-222">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ababb-222">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ababb-224">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ababb-224">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ababb-226">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ababb-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ababb-228">a.</span><span class="sxs-lookup"><span data-stu-id="ababb-228">a.</span></span> <span data-ttu-id="ababb-229">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ababb-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ababb-230">b.</span><span class="sxs-lookup"><span data-stu-id="ababb-230">b.</span></span> <span data-ttu-id="ababb-231">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ababb-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ababb-232">c.</span><span class="sxs-lookup"><span data-stu-id="ababb-232">c.</span></span> <span data-ttu-id="ababb-233">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ababb-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ababb-234">d.</span><span class="sxs-lookup"><span data-stu-id="ababb-234">d.</span></span> <span data-ttu-id="ababb-235">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ababb-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="ababb-236">Vytvoření zkušebního uživatele izolovaného prostoru Salesforce</span><span class="sxs-lookup"><span data-stu-id="ababb-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="ababb-237">V této části se uživatel volá Britta Simon vytvoří v izolovaného prostoru služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="ababb-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="ababb-238">Izolovaný prostor Salesforce podporuje zřizování za běhu, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="ababb-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="ababb-239">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="ababb-239">There is no action item for you in this section.</span></span> <span data-ttu-id="ababb-240">Pokud uživatel v izolovaném prostoru Salesforce ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Salesforce izolovaného prostoru.</span><span class="sxs-lookup"><span data-stu-id="ababb-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt tooaccess Salesforce Sandbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ababb-241">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ababb-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ababb-242">V této části povolíte tak, že udělíte přístup tooSalesforce izolovaného prostoru Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ababb-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSalesforce Sandbox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ababb-244">**tooassign tooSalesforce Britta Simon izolovaného prostoru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ababb-244">**tooassign Britta Simon tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="ababb-245">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ababb-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ababb-247">V seznamu aplikace hello vyberte **izolovaného prostoru Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="ababb-247">In hello applications list, select **Salesforce Sandbox**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="ababb-249">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ababb-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ababb-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ababb-251">Click **Add** button.</span></span> <span data-ttu-id="ababb-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ababb-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ababb-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ababb-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ababb-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ababb-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ababb-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ababb-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ababb-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ababb-257">Testing single sign-on</span></span>

<span data-ttu-id="ababb-258">Pokud chcete tootest nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ababb-258">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="ababb-259">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ababb-259">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ababb-260">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ababb-260">Additional resources</span></span>

* [<span data-ttu-id="ababb-261">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ababb-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ababb-262">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ababb-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="ababb-263">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="ababb-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

