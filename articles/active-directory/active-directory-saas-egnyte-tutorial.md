---
title: 'Kurz: Azure Active Directory integrace s Egnyte | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="0e840-103">Kurz: Azure Active Directory integrace s Egnyte</span><span class="sxs-lookup"><span data-stu-id="0e840-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="0e840-104">V tomto kurzu zjistíte, jak toointegrate Egnyte s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0e840-104">In this tutorial, you learn how toointegrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0e840-105">Integrace Egnyte s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0e840-105">Integrating Egnyte with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0e840-106">Můžete řídit ve službě Azure AD, který má přístup tooEgnyte</span><span class="sxs-lookup"><span data-stu-id="0e840-106">You can control in Azure AD who has access tooEgnyte</span></span>
- <span data-ttu-id="0e840-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEgnyte (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e840-107">You can enable your users tooautomatically get signed-on tooEgnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0e840-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0e840-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0e840-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0e840-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e840-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0e840-110">Prerequisites</span></span>

<span data-ttu-id="0e840-111">Integrace služby Azure AD s Egnyte tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0e840-111">tooconfigure Azure AD integration with Egnyte, you need hello following items:</span></span>

- <span data-ttu-id="0e840-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e840-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0e840-113">Egnyte jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0e840-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0e840-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0e840-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0e840-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0e840-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0e840-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0e840-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0e840-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e840-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0e840-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0e840-118">Scenario description</span></span>
<span data-ttu-id="0e840-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0e840-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0e840-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0e840-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0e840-121">Přidání Egnyte z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0e840-121">Adding Egnyte from hello gallery</span></span>
2. <span data-ttu-id="0e840-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0e840-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-hello-gallery"></a><span data-ttu-id="0e840-123">Přidání Egnyte z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0e840-123">Adding Egnyte from hello gallery</span></span>
<span data-ttu-id="0e840-124">tooconfigure hello integrace Egnyte do Azure AD, je nutné tooadd Egnyte hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0e840-124">tooconfigure hello integration of Egnyte into Azure AD, you need tooadd Egnyte from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0e840-125">**tooadd Egnyte z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0e840-125">**tooadd Egnyte from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e840-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0e840-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0e840-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0e840-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0e840-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0e840-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0e840-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0e840-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0e840-133">Hello vyhledávacího pole zadejte **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="0e840-133">In hello search box, type **Egnyte**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="0e840-135">Na panelu výsledků hello vyberte **Egnyte**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e840-135">In hello results panel, select **Egnyte**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0e840-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0e840-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0e840-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Egnyte podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0e840-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0e840-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Egnyte je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e840-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Egnyte is tooa user in Azure AD.</span></span> <span data-ttu-id="0e840-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Egnyte musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0e840-140">In other words, a link relationship between an Azure AD user and hello related user in Egnyte needs toobe established.</span></span>

<span data-ttu-id="0e840-141">V Egnyte, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="0e840-141">In Egnyte, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0e840-142">tooconfigure a testu Azure AD jednotné přihlašování s Egnyte, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0e840-142">tooconfigure and test Azure AD single sign-on with Egnyte, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0e840-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0e840-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0e840-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0e840-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0e840-145">**[Vytváření testovacího uživatele Egnyte](#creating-an-egnyte-test-user)**  -toohave protějšek Britta Simon v Egnyte, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0e840-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - toohave a counterpart of Britta Simon in Egnyte that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0e840-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0e840-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0e840-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0e840-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0e840-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0e840-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0e840-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0e840-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="0e840-150">**tooconfigure Azure AD jednotné přihlašování s Egnyte, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0e840-150">**tooconfigure Azure AD single sign-on with Egnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e840-151">V portálu Azure, na hello hello **Egnyte** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0e840-151">In hello Azure portal, on hello **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0e840-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0e840-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="0e840-155">Na hello **Egnyte domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0e840-155">On hello **Egnyte Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="0e840-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="0e840-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0e840-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="0e840-158">This value is not real.</span></span> <span data-ttu-id="0e840-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0e840-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="0e840-160">Obraťte se na [tým podpory Egnyte klienta](https://www.egnyte.com/corp/contact_egnyte.html) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0e840-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="0e840-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0e840-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="0e840-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0e840-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0e840-165">Na hello **Egnyte konfigurace** klikněte na tlačítko **konfigurace Egnyte** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0e840-165">On hello **Egnyte Configuration** section, click **Configure Egnyte** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0e840-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0e840-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="0e840-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Egnyte tooyour.</span><span class="sxs-lookup"><span data-stu-id="0e840-168">In a different web browser window, log in tooyour Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="0e840-169">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0e840-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="0e840-170">![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787819.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="0e840-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="0e840-171">V nabídce hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0e840-171">In hello menu, click **Settings**.</span></span>

   <span data-ttu-id="0e840-172">![Nastavení](./media/active-directory-saas-egnyte-tutorial/ic787820.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="0e840-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="0e840-173">Klikněte na tlačítko hello **konfigurace** a pak klikněte **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0e840-173">Click hello **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="0e840-174">![Zabezpečení](./media/active-directory-saas-egnyte-tutorial/ic787821.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="0e840-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="0e840-175">V hello **ověření jednotného přihlašování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0e840-175">In hello **Single Sign-On Authentication** section, perform hello following steps:</span></span>

    <span data-ttu-id="0e840-176">![Jednotné přihlašování na ověřování](./media/active-directory-saas-egnyte-tutorial/ic787822.png "jednotné přihlašování v ověřování")</span><span class="sxs-lookup"><span data-stu-id="0e840-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="0e840-177">a.</span><span class="sxs-lookup"><span data-stu-id="0e840-177">a.</span></span> <span data-ttu-id="0e840-178">Jako **ověření jednotného přihlašování**, vyberte **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="0e840-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="0e840-179">b.</span><span class="sxs-lookup"><span data-stu-id="0e840-179">b.</span></span> <span data-ttu-id="0e840-180">Jako **zprostředkovatele Identity**, vyberte **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="0e840-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="0e840-181">c.</span><span class="sxs-lookup"><span data-stu-id="0e840-181">c.</span></span> <span data-ttu-id="0e840-182">Vložení **SAML jeden přihlašování adresa URL služby** zkopírovaných z portálu Azure do hello **adresu URL pro přihlášení zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0e840-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into hello **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="0e840-183">d.</span><span class="sxs-lookup"><span data-stu-id="0e840-183">d.</span></span> <span data-ttu-id="0e840-184">Vložení **SAML Entity ID** který jste zkopírovali z portálu Azure do hello **ID entity zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0e840-184">Paste **SAML Entity ID** which you have copied from Azure portal into hello **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="0e840-185">e.</span><span class="sxs-lookup"><span data-stu-id="0e840-185">e.</span></span> <span data-ttu-id="0e840-186">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát zprostředkovatele Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0e840-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="0e840-187">f.</span><span class="sxs-lookup"><span data-stu-id="0e840-187">f.</span></span> <span data-ttu-id="0e840-188">Jako **výchozí mapování uživatelů**, vyberte **e-mailová adresa**.</span><span class="sxs-lookup"><span data-stu-id="0e840-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="0e840-189">g.</span><span class="sxs-lookup"><span data-stu-id="0e840-189">g.</span></span> <span data-ttu-id="0e840-190">Jako **použít hodnotu specifické pro doménu vystavitele**, vyberte **zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="0e840-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="0e840-191">h.</span><span class="sxs-lookup"><span data-stu-id="0e840-191">h.</span></span> <span data-ttu-id="0e840-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0e840-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0e840-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0e840-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0e840-194">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0e840-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0e840-195">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0e840-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0e840-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e840-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="0e840-197">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0e840-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0e840-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0e840-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e840-200">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0e840-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0e840-202">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0e840-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0e840-204">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="0e840-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0e840-206">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0e840-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0e840-208">a.</span><span class="sxs-lookup"><span data-stu-id="0e840-208">a.</span></span> <span data-ttu-id="0e840-209">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0e840-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0e840-210">b.</span><span class="sxs-lookup"><span data-stu-id="0e840-210">b.</span></span> <span data-ttu-id="0e840-211">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0e840-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0e840-212">c.</span><span class="sxs-lookup"><span data-stu-id="0e840-212">c.</span></span> <span data-ttu-id="0e840-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0e840-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0e840-214">d.</span><span class="sxs-lookup"><span data-stu-id="0e840-214">d.</span></span> <span data-ttu-id="0e840-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0e840-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="0e840-216">Vytváření testovacího uživatele Egnyte</span><span class="sxs-lookup"><span data-stu-id="0e840-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="0e840-217">Uživatelé toolog tooenable Azure AD v tooEgnyte, se musí být zřízená do Egnyte.</span><span class="sxs-lookup"><span data-stu-id="0e840-217">tooenable Azure AD users toolog in tooEgnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="0e840-218">V případě hello Egnyte zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="0e840-218">In hello case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="0e840-219">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0e840-219">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e840-220">Přihlaste se tooyour **Egnyte** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="0e840-220">Log in tooyour **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="0e840-221">Přejděte příliš**nastavení \> uživatelé a skupiny**.</span><span class="sxs-lookup"><span data-stu-id="0e840-221">Go too**Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="0e840-222">Klikněte na tlačítko **přidat nové uživatele**a pak vyberte typ hello uživatele chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="0e840-222">Click **Add New User**, and then select hello type of user you want tooadd.</span></span>
   
   <span data-ttu-id="0e840-223">![Uživatelé](./media/active-directory-saas-egnyte-tutorial/ic787824.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="0e840-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="0e840-224">V hello **nové standardní uživatel** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0e840-224">In hello **New Standard User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="0e840-225">![Nový uživatel standardní](./media/active-directory-saas-egnyte-tutorial/ic787825.png "nové standardní uživatel")</span><span class="sxs-lookup"><span data-stu-id="0e840-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="0e840-226">a.</span><span class="sxs-lookup"><span data-stu-id="0e840-226">a.</span></span> <span data-ttu-id="0e840-227">Typ hello **e-mailu**, **uživatelské jméno**a další podrobnosti chcete tooprovision platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0e840-227">Type hello **Email**, **Username**, and other details of a valid Azure Active Directory account you want tooprovision.</span></span>
   
   <span data-ttu-id="0e840-228">b.</span><span class="sxs-lookup"><span data-stu-id="0e840-228">b.</span></span> <span data-ttu-id="0e840-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0e840-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="0e840-230">Držitel účtu Azure Active Directory Hello obdrží e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="0e840-230">hello Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="0e840-231">Můžete použít všechny ostatní Egnyte uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Egnyte tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="0e840-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0e840-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0e840-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0e840-233">V této části povolíte tak, že udělíte přístup tooEgnyte toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0e840-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEgnyte.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0e840-235">**tooassign Britta Simon tooEgnyte, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0e840-235">**tooassign Britta Simon tooEgnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e840-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0e840-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0e840-238">V seznamu aplikace hello vyberte **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="0e840-238">In hello applications list, select **Egnyte**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="0e840-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0e840-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0e840-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0e840-242">Click **Add** button.</span></span> <span data-ttu-id="0e840-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0e840-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0e840-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0e840-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0e840-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0e840-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0e840-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0e840-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0e840-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0e840-248">Testing single sign-on</span></span>

<span data-ttu-id="0e840-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0e840-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0e840-250">Když kliknete na dlaždici Egnyte hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Egnyte aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e840-250">When you click hello Egnyte tile in hello Access Panel, you should get automatically signed-on tooyour Egnyte application.</span></span>
<span data-ttu-id="0e840-251">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0e840-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0e840-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0e840-252">Additional resources</span></span>

* [<span data-ttu-id="0e840-253">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e840-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0e840-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0e840-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

