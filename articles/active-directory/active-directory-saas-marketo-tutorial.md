---
title: 'Kurz: Azure Active Directory integrace s Marketo | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="e1624-103">Kurz: Azure Active Directory integrace s Marketo</span><span class="sxs-lookup"><span data-stu-id="e1624-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="e1624-104">V tomto kurzu zjistíte, jak toointegrate Marketo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e1624-104">In this tutorial, you learn how toointegrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e1624-105">Integrace služby Marketo s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e1624-105">Integrating Marketo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e1624-106">Můžete řídit ve službě Azure AD, který má přístup tooMarketo</span><span class="sxs-lookup"><span data-stu-id="e1624-106">You can control in Azure AD who has access tooMarketo</span></span>
- <span data-ttu-id="e1624-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMarketo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1624-107">You can enable your users tooautomatically get signed-on tooMarketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e1624-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e1624-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e1624-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e1624-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1624-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e1624-110">Prerequisites</span></span>

<span data-ttu-id="e1624-111">tooconfigure integrace Azure AD s Marketo, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="e1624-111">tooconfigure Azure AD integration with Marketo, you need hello following items:</span></span>

- <span data-ttu-id="e1624-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1624-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e1624-113">Marketo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e1624-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e1624-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1624-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e1624-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e1624-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e1624-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e1624-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e1624-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e1624-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e1624-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e1624-118">Scenario description</span></span>
<span data-ttu-id="e1624-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e1624-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e1624-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e1624-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e1624-121">Přidání Marketo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e1624-121">Adding Marketo from hello gallery</span></span>
2. <span data-ttu-id="e1624-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1624-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-hello-gallery"></a><span data-ttu-id="e1624-123">Přidání Marketo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="e1624-123">Adding Marketo from hello gallery</span></span>
<span data-ttu-id="e1624-124">tooconfigure hello integrace Marketo do Azure AD, je nutné tooadd Marketo hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e1624-124">tooconfigure hello integration of Marketo into Azure AD, you need tooadd Marketo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e1624-125">**tooadd Marketo z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e1624-125">**tooadd Marketo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1624-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e1624-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e1624-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e1624-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e1624-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1624-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e1624-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1624-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e1624-133">Hello vyhledávacího pole zadejte **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="e1624-133">In hello search box, type **Marketo**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="e1624-135">Na panelu výsledků hello vyberte **Marketo**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1624-135">In hello results panel, select **Marketo**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e1624-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1624-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e1624-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Marketo podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e1624-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e1624-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Marketo je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e1624-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Marketo is tooa user in Azure AD.</span></span> <span data-ttu-id="e1624-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Marketo musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="e1624-140">In other words, a link relationship between an Azure AD user and hello related user in Marketo needs toobe established.</span></span>

<span data-ttu-id="e1624-141">V Marketo, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="e1624-141">In Marketo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e1624-142">tooconfigure a testu Azure AD jednotné přihlašování s Marketo, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="e1624-142">tooconfigure and test Azure AD single sign-on with Marketo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e1624-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="e1624-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e1624-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e1624-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e1624-145">**[Vytvoření zkušebního uživatele Marketo](#creating-a-marketo-test-user)**  -toohave protějšek Britta Simon v Marketo, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e1624-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - toohave a counterpart of Britta Simon in Marketo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e1624-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1624-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e1624-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="e1624-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e1624-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1624-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e1624-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Marketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="e1624-150">**tooconfigure Azure AD jednotné přihlašování pomocí služby Marketo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e1624-150">**tooconfigure Azure AD single sign-on with Marketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1624-151">V portálu Azure, na hello hello **Marketo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e1624-151">In hello Azure portal, on hello **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e1624-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1624-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="e1624-155">Na hello **Marketo domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="e1624-155">On hello **Marketo Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="e1624-157">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-157">a.</span></span> <span data-ttu-id="e1624-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="e1624-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="e1624-159">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-159">b.</span></span> <span data-ttu-id="e1624-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="e1624-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e1624-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="e1624-161">These values are not real.</span></span> <span data-ttu-id="e1624-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e1624-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="e1624-163">Obraťte se na [tým podpory Marketo](http://investors.marketo.com/contactus.cfm) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e1624-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) tooget these values.</span></span>
 
4. <span data-ttu-id="e1624-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e1624-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="e1624-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1624-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e1624-168">Na hello **Marketo konfigurace** klikněte na tlačítko **konfigurace Marketo** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e1624-168">On hello **Marketo Configuration** section, click **Configure Marketo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e1624-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e1624-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="e1624-171">tooget Munchkin Id aplikace, přihlaste se pomocí přihlašovacích údajů správce tooMarketo a proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="e1624-171">tooget Munchkin Id of your application, log in tooMarketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="e1624-172">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-172">a.</span></span> <span data-ttu-id="e1624-173">Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-173">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="e1624-174">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-174">b.</span></span> <span data-ttu-id="e1624-175">Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-175">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="e1624-177">c.</span><span class="sxs-lookup"><span data-stu-id="e1624-177">c.</span></span> <span data-ttu-id="e1624-178">Přejděte toohello integrace nabídky a klikněte na tlačítko hello **Munchkin odkaz**.</span><span class="sxs-lookup"><span data-stu-id="e1624-178">Navigate toohello Integration menu and click hello **Munchkin link**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="e1624-180">d.</span><span class="sxs-lookup"><span data-stu-id="e1624-180">d.</span></span> <span data-ttu-id="e1624-181">Zkopírujte hello Munchkin Id zobrazí na úvodní obrazovka a dokončit vaše adresa URL odpovědi v Průvodci konfigurací hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e1624-181">Copy hello Munchkin Id shown on hello screen and complete your Reply URL in hello Azure AD configuration wizard.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="e1624-183">tooconfigure hello jednotné přihlašování v aplikaci hello, postupujte podle hello následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="e1624-183">tooconfigure hello SSO in hello application, follow hello below steps:</span></span>
   
    <span data-ttu-id="e1624-184">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-184">a.</span></span> <span data-ttu-id="e1624-185">Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-185">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="e1624-186">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-186">b.</span></span> <span data-ttu-id="e1624-187">Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-187">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="e1624-189">c.</span><span class="sxs-lookup"><span data-stu-id="e1624-189">c.</span></span> <span data-ttu-id="e1624-190">Přejděte toohello integrace nabídky a klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e1624-190">Navigate toohello Integration menu and click **Single Sign On**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="e1624-192">d.</span><span class="sxs-lookup"><span data-stu-id="e1624-192">d.</span></span> <span data-ttu-id="e1624-193">Klikněte na tlačítko tooenable hello SAML nastavení **upravit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1624-193">tooenable hello SAML Settings, click **Edit** button.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="e1624-195">e.</span><span class="sxs-lookup"><span data-stu-id="e1624-195">e.</span></span> <span data-ttu-id="e1624-196">**Povolit** nastavení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1624-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="e1624-197">f.</span><span class="sxs-lookup"><span data-stu-id="e1624-197">f.</span></span> <span data-ttu-id="e1624-198">Vložení hello **SAML Entity ID**, v hello **ID vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1624-198">Paste hello **SAML Entity ID**, in hello **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="e1624-199">g.</span><span class="sxs-lookup"><span data-stu-id="e1624-199">g.</span></span> <span data-ttu-id="e1624-200">V hello **Entity ID** textovému poli, zadejte adresu URL hello jako `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="e1624-200">In hello **Entity ID** textbox, enter hello URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="e1624-201">h.</span><span class="sxs-lookup"><span data-stu-id="e1624-201">h.</span></span> <span data-ttu-id="e1624-202">Vyberte hello umístění ID uživatele jako **název identifikátor elementu**.</span><span class="sxs-lookup"><span data-stu-id="e1624-202">Select hello User ID Location as **Name Identifier element**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="e1624-204">Pokud není uživatelský identifikátor UPN hodnota a hodnota hello změnit na kartě atribut hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-204">If your User Identifier is not UPN value then change hello value in hello Attribute tab.</span></span>
   
    <span data-ttu-id="e1624-205">i.</span><span class="sxs-lookup"><span data-stu-id="e1624-205">i.</span></span> <span data-ttu-id="e1624-206">Nahrajte hello certifikát, který jste si stáhli z Průvodce konfigurací služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e1624-206">Upload hello certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="e1624-207">**Uložit** hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="e1624-207">**Save** hello settings.</span></span>
   
    <span data-ttu-id="e1624-208">j.</span><span class="sxs-lookup"><span data-stu-id="e1624-208">j.</span></span> <span data-ttu-id="e1624-209">Upravte nastavení přesměrování stránky hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-209">Edit hello Redirect Pages settings.</span></span>
   
    <span data-ttu-id="e1624-210">kB.</span><span class="sxs-lookup"><span data-stu-id="e1624-210">k.</span></span> <span data-ttu-id="e1624-211">Vložení hello **SAML jeden přihlašování adresa URL služby** v hello **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1624-211">Paste hello **SAML Single Sign-On Service URL** in hello **Login URL** textbox.</span></span>
   
    <span data-ttu-id="e1624-212">l.</span><span class="sxs-lookup"><span data-stu-id="e1624-212">l.</span></span> <span data-ttu-id="e1624-213">Vložení hello **Sign-Out URL** v hello **adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e1624-213">Paste hello **Sign-Out URL** in hello **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="e1624-214">m.</span><span class="sxs-lookup"><span data-stu-id="e1624-214">m.</span></span> <span data-ttu-id="e1624-215">V hello **chyba URL**, kopie vašeho **adresu URL instance Marketo** a klikněte na tlačítko **Uložit** tlačítko toosave nastavení.</span><span class="sxs-lookup"><span data-stu-id="e1624-215">In hello **Error URL**, copy your **Marketo instance URL** and click **Save** button toosave settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="e1624-217">tooenable hello jednotného přihlašování pro uživatele, dokončení hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="e1624-217">tooenable hello SSO for users, complete hello following actions:</span></span>
   
    <span data-ttu-id="e1624-218">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-218">a.</span></span> <span data-ttu-id="e1624-219">Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-219">Log in tooMarketo app using admin credentials.</span></span>
   
    <span data-ttu-id="e1624-220">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-220">b.</span></span> <span data-ttu-id="e1624-221">Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-221">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="e1624-223">c.</span><span class="sxs-lookup"><span data-stu-id="e1624-223">c.</span></span> <span data-ttu-id="e1624-224">Přejděte toohello **zabezpečení** nabídky a klikněte na tlačítko **nastavení přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="e1624-224">Navigate toohello **Security** menu and click **Login Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="e1624-226">d.</span><span class="sxs-lookup"><span data-stu-id="e1624-226">d.</span></span> <span data-ttu-id="e1624-227">Zkontrolujte hello **vyžadovat jednotné přihlašování** možnost a **Uložit** hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="e1624-227">Check hello **Require SSO** option and **Save** hello settings.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="e1624-229">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="e1624-229">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e1624-230">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-230">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e1624-231">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e1624-231">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e1624-232">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e1624-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="e1624-233">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-233">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e1624-235">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e1624-235">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1624-236">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e1624-236">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e1624-238">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e1624-238">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e1624-240">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="e1624-240">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e1624-242">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e1624-242">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e1624-244">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-244">a.</span></span> <span data-ttu-id="e1624-245">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e1624-245">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e1624-246">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-246">b.</span></span> <span data-ttu-id="e1624-247">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e1624-247">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e1624-248">c.</span><span class="sxs-lookup"><span data-stu-id="e1624-248">c.</span></span> <span data-ttu-id="e1624-249">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e1624-249">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e1624-250">d.</span><span class="sxs-lookup"><span data-stu-id="e1624-250">d.</span></span> <span data-ttu-id="e1624-251">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e1624-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="e1624-252">Vytvoření zkušebního uživatele Marketo</span><span class="sxs-lookup"><span data-stu-id="e1624-252">Creating a Marketo test user</span></span>

<span data-ttu-id="e1624-253">V této části vytvoříte uživatele volal Britta Simon v Marketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="e1624-254">postupujte podle těchto kroků toocreate uživatele v Marketo platformy.</span><span class="sxs-lookup"><span data-stu-id="e1624-254">follow these steps toocreate a user in Marketo platform.</span></span>

1. <span data-ttu-id="e1624-255">Přihlaste se pomocí přihlašovacích údajů správce aplikace tooMarketo.</span><span class="sxs-lookup"><span data-stu-id="e1624-255">Log in tooMarketo app using admin credentials.</span></span>

2. <span data-ttu-id="e1624-256">Klikněte na tlačítko hello **správce** tlačítko v horním navigačním podokně hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-256">Click hello **Admin** button on hello top navigation pane.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="e1624-258">Přejděte toohello **zabezpečení** nabídky a klikněte na tlačítko **uživatelé a role**</span><span class="sxs-lookup"><span data-stu-id="e1624-258">Navigate toohello **Security** menu and click **Users & Roles**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="e1624-260">Klikněte na tlačítko hello **pozvat nového uživatele** odkaz na kartě Uživatelé hello</span><span class="sxs-lookup"><span data-stu-id="e1624-260">Click hello **Invite New User** link on hello Users tab</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="e1624-262">V následující informace o aktualizaci hello pozvat nového uživatele Průvodce výplně hello</span><span class="sxs-lookup"><span data-stu-id="e1624-262">In hello Invite New User wizard fill hello following information</span></span>
   
    <span data-ttu-id="e1624-263">a.</span><span class="sxs-lookup"><span data-stu-id="e1624-263">a.</span></span> <span data-ttu-id="e1624-264">Zadejte uživatele hello **e-mailu** adresu v textovém poli hello</span><span class="sxs-lookup"><span data-stu-id="e1624-264">Enter hello user **Email** address in hello textbox</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="e1624-266">b.</span><span class="sxs-lookup"><span data-stu-id="e1624-266">b.</span></span> <span data-ttu-id="e1624-267">Zadejte hello **křestní jméno** v textovém poli hello</span><span class="sxs-lookup"><span data-stu-id="e1624-267">Enter hello **First Name** in hello textbox</span></span>
   
    <span data-ttu-id="e1624-268">c.</span><span class="sxs-lookup"><span data-stu-id="e1624-268">c.</span></span> <span data-ttu-id="e1624-269">Zadejte hello **příjmení** v textovém poli hello</span><span class="sxs-lookup"><span data-stu-id="e1624-269">Enter hello **Last Name**  in hello textbox</span></span>
   
    <span data-ttu-id="e1624-270">d.</span><span class="sxs-lookup"><span data-stu-id="e1624-270">d.</span></span> <span data-ttu-id="e1624-271">Klikněte na **Další**</span><span class="sxs-lookup"><span data-stu-id="e1624-271">Click **Next**</span></span>

6. <span data-ttu-id="e1624-272">V hello **oprávnění** karty, vyberte hello **userRoles** a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="e1624-272">In hello **Permissions** tab, select hello **userRoles** and click **Next**</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="e1624-274">Klikněte na tlačítko hello **odeslat** tlačítko toosend hello uživatele Pozvánka</span><span class="sxs-lookup"><span data-stu-id="e1624-274">Click hello **Send** button toosend hello user invitation</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="e1624-276">Uživatel obdrží e-mailové oznámení hello a má tooclick hello propojení a změňte účet hello tooactivate hello heslo.</span><span class="sxs-lookup"><span data-stu-id="e1624-276">User receives hello email notification and has tooclick hello link and change hello password tooactivate hello account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e1624-277">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="e1624-277">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e1624-278">V této části povolíte tak, že udělíte přístup tooMarketo toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e1624-278">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMarketo.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e1624-280">**tooassign Britta Simon tooMarketo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="e1624-280">**tooassign Britta Simon tooMarketo, perform hello following steps:**</span></span>

1. <span data-ttu-id="e1624-281">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e1624-281">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e1624-283">V seznamu aplikace hello vyberte **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="e1624-283">In hello applications list, select **Marketo**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="e1624-285">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e1624-285">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e1624-287">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e1624-287">Click **Add** button.</span></span> <span data-ttu-id="e1624-288">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1624-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e1624-290">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="e1624-290">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e1624-291">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1624-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e1624-292">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e1624-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e1624-293">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e1624-293">Testing single sign-on</span></span>

<span data-ttu-id="e1624-294">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e1624-294">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e1624-295">Když kliknete na dlaždici Marketo hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Marketo aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1624-295">When you click hello Marketo tile in hello Access Panel, you should get automatically signed-on tooyour Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1624-296">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e1624-296">Additional resources</span></span>

* [<span data-ttu-id="e1624-297">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1624-297">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e1624-298">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e1624-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

