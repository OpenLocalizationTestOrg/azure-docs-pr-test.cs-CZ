---
title: 'Kurz: Azure Active Directory integrace s Mozy Enterprise | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Mozy Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: bab0df4f3621b784cd8edfda3c8e10fe5a7ced9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="610ef-103">Kurz: Azure Active Directory integrace s Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="610ef-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="610ef-104">V tomto kurzu zjistíte, jak toointegrate Mozy Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="610ef-104">In this tutorial, you learn how toointegrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="610ef-105">Integrace Mozy Enterprise s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="610ef-105">Integrating Mozy Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="610ef-106">Můžete řídit ve službě Azure AD, který má přístup tooMozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="610ef-106">You can control in Azure AD who has access tooMozy Enterprise</span></span>
- <span data-ttu-id="610ef-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMozy Enterprise (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="610ef-107">You can enable your users tooautomatically get signed-on tooMozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="610ef-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="610ef-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="610ef-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="610ef-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="610ef-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="610ef-110">Prerequisites</span></span>

<span data-ttu-id="610ef-111">tooconfigure integrace Azure AD s Mozy Enterprise, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="610ef-111">tooconfigure Azure AD integration with Mozy Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="610ef-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="610ef-112">An Azure AD subscription</span></span>
- <span data-ttu-id="610ef-113">Mozy Enterprise jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="610ef-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="610ef-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="610ef-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="610ef-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="610ef-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="610ef-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="610ef-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="610ef-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="610ef-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="610ef-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="610ef-118">Scenario description</span></span>
<span data-ttu-id="610ef-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="610ef-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="610ef-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="610ef-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="610ef-121">Přidání Mozy Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="610ef-121">Adding Mozy Enterprise from hello gallery</span></span>
2. <span data-ttu-id="610ef-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="610ef-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-hello-gallery"></a><span data-ttu-id="610ef-123">Přidání Mozy Enterprise z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="610ef-123">Adding Mozy Enterprise from hello gallery</span></span>
<span data-ttu-id="610ef-124">tooconfigure hello integrace Mozy Enterprise do Azure AD, je nutné tooadd Mozy Enterprise hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="610ef-124">tooconfigure hello integration of Mozy Enterprise into Azure AD, you need tooadd Mozy Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="610ef-125">**tooadd Mozy Enterprise z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="610ef-125">**tooadd Mozy Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="610ef-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="610ef-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="610ef-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="610ef-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="610ef-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="610ef-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="610ef-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="610ef-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="610ef-133">Hello vyhledávacího pole zadejte **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="610ef-133">In hello search box, type **Mozy Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="610ef-135">Na panelu výsledků hello vyberte **Mozy Enterprise**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="610ef-135">In hello results panel, select **Mozy Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="610ef-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="610ef-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="610ef-138">V této části můžete nakonfigurovat, testovací Azure AD jednotného přihlašování k Mozy organizace podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="610ef-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="610ef-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello protějšku uživatele v podniku Mozy je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="610ef-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mozy Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="610ef-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v podniku Mozy musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="610ef-140">In other words, a link relationship between an Azure AD user and hello related user in Mozy Enterprise needs toobe established.</span></span>

<span data-ttu-id="610ef-141">V Mozy Enterprise, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="610ef-141">In Mozy Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="610ef-142">tooconfigure a testování Azure AD jednotného přihlašování k Mozy organizace, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="610ef-142">tooconfigure and test Azure AD single sign-on with Mozy Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="610ef-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="610ef-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="610ef-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="610ef-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="610ef-145">**[Vytvoření zkušebního uživatele Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  -toohave protějšek Britta Simon v Mozy organizace, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="610ef-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - toohave a counterpart of Britta Simon in Mozy Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="610ef-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="610ef-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="610ef-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="610ef-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="610ef-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="610ef-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="610ef-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="610ef-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="610ef-150">**tooconfigure Azure AD jednotné přihlašování s Mozy Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="610ef-150">**tooconfigure Azure AD single sign-on with Mozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="610ef-151">V portálu Azure, na hello hello **Mozy Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="610ef-151">In hello Azure portal, on hello **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="610ef-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="610ef-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="610ef-155">Na hello **Mozy podnikové domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="610ef-155">On hello **Mozy Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="610ef-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="610ef-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="610ef-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="610ef-158">This value is not real.</span></span> <span data-ttu-id="610ef-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="610ef-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="610ef-160">Obraťte se na [tým podpory klient systému Enterprise Mozy](http://support.mozy.com/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="610ef-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) tooget this value.</span></span>

4. <span data-ttu-id="610ef-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="610ef-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="610ef-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="610ef-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="610ef-165">Na hello **Mozy podniková konfigurace** klikněte na tlačítko **konfigurace Mozy Enterprise** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="610ef-165">On hello **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="610ef-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="610ef-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="610ef-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="610ef-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="610ef-169">V hello **konfigurace** klikněte na tlačítko **zásady ověřování**.</span><span class="sxs-lookup"><span data-stu-id="610ef-169">In hello **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="610ef-170">![Zásady ověřování](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "zásady ověřování")</span><span class="sxs-lookup"><span data-stu-id="610ef-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="610ef-171">Na hello **zásady ověřování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="610ef-171">On hello **Authentication Policy** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="610ef-172">![Zásady ověřování](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "zásady ověřování")</span><span class="sxs-lookup"><span data-stu-id="610ef-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="610ef-173">a.</span><span class="sxs-lookup"><span data-stu-id="610ef-173">a.</span></span> <span data-ttu-id="610ef-174">Vyberte **adresářová služba** jako **zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="610ef-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="610ef-175">b.</span><span class="sxs-lookup"><span data-stu-id="610ef-175">b.</span></span> <span data-ttu-id="610ef-176">Vyberte **použít nabízenou LDAP**.</span><span class="sxs-lookup"><span data-stu-id="610ef-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="610ef-177">c.</span><span class="sxs-lookup"><span data-stu-id="610ef-177">c.</span></span> <span data-ttu-id="610ef-178">Klikněte na tlačítko hello **ověřování SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="610ef-178">Click hello **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="610ef-179">d.</span><span class="sxs-lookup"><span data-stu-id="610ef-179">d.</span></span> <span data-ttu-id="610ef-180">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **adresy URL ověřování** textové pole.</span><span class="sxs-lookup"><span data-stu-id="610ef-180">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="610ef-181">e.</span><span class="sxs-lookup"><span data-stu-id="610ef-181">e.</span></span> <span data-ttu-id="610ef-182">Vložení **SAML Entity ID**, který jste zkopírovali z hello portálu Azure do hello **koncový bod SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="610ef-182">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="610ef-183">f.</span><span class="sxs-lookup"><span data-stu-id="610ef-183">f.</span></span> <span data-ttu-id="610ef-184">Stažený kódovaného certifikátu kódování base-64 otevřete v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="610ef-184">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="610ef-185">g.</span><span class="sxs-lookup"><span data-stu-id="610ef-185">g.</span></span> <span data-ttu-id="610ef-186">Vyberte **povolit jednotné přihlašování pro Admins toolog se pomocí svých přihlašovacích údajů sítě**.</span><span class="sxs-lookup"><span data-stu-id="610ef-186">Select **Enable SSO for Admins toolog in with their network credentials**.</span></span>
   
   <span data-ttu-id="610ef-187">h.</span><span class="sxs-lookup"><span data-stu-id="610ef-187">h.</span></span> <span data-ttu-id="610ef-188">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="610ef-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="610ef-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="610ef-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="610ef-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="610ef-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="610ef-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="610ef-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="610ef-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="610ef-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="610ef-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="610ef-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="610ef-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="610ef-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="610ef-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="610ef-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="610ef-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="610ef-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="610ef-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="610ef-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="610ef-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="610ef-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="610ef-204">a.</span><span class="sxs-lookup"><span data-stu-id="610ef-204">a.</span></span> <span data-ttu-id="610ef-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="610ef-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="610ef-206">b.</span><span class="sxs-lookup"><span data-stu-id="610ef-206">b.</span></span> <span data-ttu-id="610ef-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="610ef-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="610ef-208">c.</span><span class="sxs-lookup"><span data-stu-id="610ef-208">c.</span></span> <span data-ttu-id="610ef-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="610ef-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="610ef-210">d.</span><span class="sxs-lookup"><span data-stu-id="610ef-210">d.</span></span> <span data-ttu-id="610ef-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="610ef-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="610ef-212">Vytvoření zkušebního uživatele Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="610ef-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="610ef-213">V pořadí tooenable Azure AD Uživatelé toolog do podnikového Mozy musí být zřízená do Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="610ef-213">In order tooenable Azure AD users toolog into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="610ef-214">V případě hello Mozy podniku zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="610ef-214">In hello case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="610ef-215">Můžete použít všechny ostatní Mozy Enterprise uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Mozy Enterprise tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="610ef-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise tooprovision AAD user accounts.</span></span>

<span data-ttu-id="610ef-216">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="610ef-216">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="610ef-217">Přihlaste se tooyour **Mozy Enterprise** klienta.</span><span class="sxs-lookup"><span data-stu-id="610ef-217">Log in tooyour **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="610ef-218">Klikněte na tlačítko **uživatelé**a potom klikněte na **přidat nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="610ef-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="610ef-219">![Uživatelé](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="610ef-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="610ef-220">Hello **přidat nové uživatele** možnost se zobrazí pouze pouze v případě **Mozy** je vybraná jako zprostředkovatel hello pod **zásady ověřování**.</span><span class="sxs-lookup"><span data-stu-id="610ef-220">hello **Add New User** option is only displayed only if **Mozy** is selected as hello provider under **Authentication policy**.</span></span> <span data-ttu-id="610ef-221">Pokud je nakonfigurované ověřování SAML, pak hello uživatelé jsou automaticky přidá na jejich první přihlášení pomocí jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="610ef-221">If SAML Authentication is configured, then hello users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="610ef-222">V uživatelském dialogu Nový hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="610ef-222">On hello new user dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="610ef-223">![Přidání uživatelů](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="610ef-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="610ef-224">a.</span><span class="sxs-lookup"><span data-stu-id="610ef-224">a.</span></span> <span data-ttu-id="610ef-225">Z hello **zvolte skupinu** seznamu, vyberte skupinu.</span><span class="sxs-lookup"><span data-stu-id="610ef-225">From hello **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="610ef-226">b.</span><span class="sxs-lookup"><span data-stu-id="610ef-226">b.</span></span> <span data-ttu-id="610ef-227">Z hello **jaký typ uživatele** seznamu, vyberte typ.</span><span class="sxs-lookup"><span data-stu-id="610ef-227">From hello **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="610ef-228">c.</span><span class="sxs-lookup"><span data-stu-id="610ef-228">c.</span></span> <span data-ttu-id="610ef-229">V hello **uživatelské jméno** textovému poli, název typu hello uživatele hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="610ef-229">In hello **Username** textbox, type hello name of hello Azure AD user.</span></span>
   
   <span data-ttu-id="610ef-230">d.</span><span class="sxs-lookup"><span data-stu-id="610ef-230">d.</span></span> <span data-ttu-id="610ef-231">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu uživatele hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="610ef-231">In hello **Email** textbox, type hello email address of hello Azure AD user.</span></span>
   
   <span data-ttu-id="610ef-232">e.</span><span class="sxs-lookup"><span data-stu-id="610ef-232">e.</span></span> <span data-ttu-id="610ef-233">Vyberte **odeslat e-mail uživatele instrukce**.</span><span class="sxs-lookup"><span data-stu-id="610ef-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="610ef-234">f.</span><span class="sxs-lookup"><span data-stu-id="610ef-234">f.</span></span> <span data-ttu-id="610ef-235">Klikněte na tlačítko **přidat jiní uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="610ef-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="610ef-236">Po vytvoření uživatele hello, e-mailu odešle toohello uživatele Azure AD, která zahrnuje účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="610ef-236">After creating hello user, an email will be sent toohello Azure AD user that includes a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="610ef-237">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="610ef-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="610ef-238">V této části povolíte tak, že udělíte přístup tooMozy Enterprise toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="610ef-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMozy Enterprise.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="610ef-240">**tooassign Britta Simon tooMozy Enterprise, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="610ef-240">**tooassign Britta Simon tooMozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="610ef-241">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="610ef-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="610ef-243">V seznamu aplikace hello vyberte **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="610ef-243">In hello applications list, select **Mozy Enterprise**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="610ef-245">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="610ef-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="610ef-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="610ef-247">Click **Add** button.</span></span> <span data-ttu-id="610ef-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="610ef-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="610ef-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="610ef-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="610ef-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="610ef-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="610ef-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="610ef-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="610ef-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="610ef-253">Testing single sign-on</span></span>

<span data-ttu-id="610ef-254">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="610ef-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="610ef-255">Po kliknutí na tlačítko hello Mozy Enterprise dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku Mozy podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="610ef-255">When you click hello Mozy Enterprise tile in hello Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="610ef-256">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="610ef-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="610ef-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="610ef-257">Additional resources</span></span>

* [<span data-ttu-id="610ef-258">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="610ef-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="610ef-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="610ef-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

