---
title: 'Kurz: Azure Active Directory integrace s Gigya | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Gigya."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 1992c5ad09b097563377a488fbf5a375f6511020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a><span data-ttu-id="0398b-103">Kurz: Azure Active Directory integrace s Gigya</span><span class="sxs-lookup"><span data-stu-id="0398b-103">Tutorial: Azure Active Directory integration with Gigya</span></span>

<span data-ttu-id="0398b-104">V tomto kurzu zjistíte, jak toointegrate Gigya s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0398b-104">In this tutorial, you learn how toointegrate Gigya with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0398b-105">Integrace Gigya s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0398b-105">Integrating Gigya with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0398b-106">Můžete řídit ve službě Azure AD, který má přístup tooGigya</span><span class="sxs-lookup"><span data-stu-id="0398b-106">You can control in Azure AD who has access tooGigya</span></span>
- <span data-ttu-id="0398b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooGigya (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0398b-107">You can enable your users tooautomatically get signed-on tooGigya (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0398b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0398b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0398b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0398b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0398b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0398b-110">Prerequisites</span></span>

<span data-ttu-id="0398b-111">Integrace služby Azure AD s Gigya tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0398b-111">tooconfigure Azure AD integration with Gigya, you need hello following items:</span></span>

- <span data-ttu-id="0398b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0398b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0398b-113">Gigya jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0398b-113">A Gigya single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0398b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0398b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0398b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0398b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0398b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0398b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0398b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0398b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0398b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0398b-118">Scenario description</span></span>
<span data-ttu-id="0398b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0398b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0398b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0398b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0398b-121">Přidání Gigya z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0398b-121">Adding Gigya from hello gallery</span></span>
2. <span data-ttu-id="0398b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0398b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gigya-from-hello-gallery"></a><span data-ttu-id="0398b-123">Přidání Gigya z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0398b-123">Adding Gigya from hello gallery</span></span>
<span data-ttu-id="0398b-124">tooconfigure hello integrace Gigya do Azure AD, je nutné tooadd Gigya hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0398b-124">tooconfigure hello integration of Gigya into Azure AD, you need tooadd Gigya from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0398b-125">**tooadd Gigya z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0398b-125">**tooadd Gigya from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0398b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0398b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0398b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0398b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0398b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0398b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0398b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0398b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0398b-133">Hello vyhledávacího pole zadejte **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="0398b-133">In hello search box, type **Gigya**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. <span data-ttu-id="0398b-135">Na panelu výsledků hello vyberte **Gigya**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0398b-135">In hello results panel, select **Gigya**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0398b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0398b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0398b-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Gigya podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="0398b-138">In this section, you configure and test Azure AD single sign-on with Gigya based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0398b-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Gigya je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0398b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Gigya is tooa user in Azure AD.</span></span> <span data-ttu-id="0398b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Gigya musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0398b-140">In other words, a link relationship between an Azure AD user and hello related user in Gigya needs toobe established.</span></span>

<span data-ttu-id="0398b-141">V Gigya, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="0398b-141">In Gigya, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0398b-142">tooconfigure a testu Azure AD jednotné přihlašování s Gigya, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0398b-142">tooconfigure and test Azure AD single sign-on with Gigya, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0398b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0398b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0398b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0398b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0398b-145">**[Vytvoření zkušebního uživatele Gigya](#creating-a-gigya-test-user)**  -toohave protějšek Britta Simon v Gigya, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0398b-145">**[Creating a Gigya test user](#creating-a-gigya-test-user)** - toohave a counterpart of Britta Simon in Gigya that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0398b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0398b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0398b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0398b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0398b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0398b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0398b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Gigya.</span><span class="sxs-lookup"><span data-stu-id="0398b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Gigya application.</span></span>

<span data-ttu-id="0398b-150">**tooconfigure Azure AD jednotné přihlašování s Gigya, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0398b-150">**tooconfigure Azure AD single sign-on with Gigya, perform hello following steps:**</span></span>

1. <span data-ttu-id="0398b-151">V portálu Azure, na hello hello **Gigya** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0398b-151">In hello Azure portal, on hello **Gigya** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0398b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0398b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. <span data-ttu-id="0398b-155">Na hello **Gigya domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0398b-155">On hello **Gigya Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    <span data-ttu-id="0398b-157">a.</span><span class="sxs-lookup"><span data-stu-id="0398b-157">a.</span></span> <span data-ttu-id="0398b-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<companyname>.gigya.com`</span><span class="sxs-lookup"><span data-stu-id="0398b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `http://<companyname>.gigya.com`</span></span>

    <span data-ttu-id="0398b-159">b.</span><span class="sxs-lookup"><span data-stu-id="0398b-159">b.</span></span> <span data-ttu-id="0398b-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://fidm.gigya.com/saml/v2.0/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="0398b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://fidm.gigya.com/saml/v2.0/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0398b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="0398b-161">These values are not real.</span></span> <span data-ttu-id="0398b-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="0398b-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0398b-163">Obraťte se na [tým podpory Gigya klienta](https://www.gigya.com/support-policy/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0398b-163">Contact [Gigya Client support team](https://www.gigya.com/support-policy/) tooget these values.</span></span> 
 
4. <span data-ttu-id="0398b-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0398b-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. <span data-ttu-id="0398b-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0398b-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0398b-168">Na hello **Gigya konfigurace** klikněte na tlačítko **konfigurace Gigya** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0398b-168">On hello **Gigya Configuration** section, click **Configure Gigya** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="0398b-169">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0398b-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. <span data-ttu-id="0398b-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Gigya.</span><span class="sxs-lookup"><span data-stu-id="0398b-171">In a different web browser window, log into your Gigya company site as an administrator.</span></span>

8. <span data-ttu-id="0398b-172">Přejděte příliš**nastavení \> SAML přihlášení**a potom klikněte na hello **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0398b-172">Go too**Settings \> SAML Login**, and then click hello **Add** button.</span></span>
   
    <span data-ttu-id="0398b-173">![Přihlašování SAML](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML přihlášení")</span><span class="sxs-lookup"><span data-stu-id="0398b-173">![SAML Login](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML Login")</span></span>

9. <span data-ttu-id="0398b-174">V hello **SAML přihlášení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0398b-174">In hello **SAML Login** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="0398b-175">![Konfigurace SAML](./media/active-directory-saas-gigya-tutorial/ic789533.png "konfigurace SAML")</span><span class="sxs-lookup"><span data-stu-id="0398b-175">![SAML Configuration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML Configuration")</span></span>
   
    <span data-ttu-id="0398b-176">a.</span><span class="sxs-lookup"><span data-stu-id="0398b-176">a.</span></span> <span data-ttu-id="0398b-177">V hello **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0398b-177">In hello **Name** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="0398b-178">b.</span><span class="sxs-lookup"><span data-stu-id="0398b-178">b.</span></span> <span data-ttu-id="0398b-179">V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0398b-179">In **Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure Portal.</span></span> 
   
    <span data-ttu-id="0398b-180">c.</span><span class="sxs-lookup"><span data-stu-id="0398b-180">c.</span></span> <span data-ttu-id="0398b-181">V **jeden přihlašování adresa URL služby** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0398b-181">In **Single Sign-On Service URL** textbox, paste hello value of **Single Sign-On Service URL** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="0398b-182">d.</span><span class="sxs-lookup"><span data-stu-id="0398b-182">d.</span></span> <span data-ttu-id="0398b-183">V **formát ID názvu** textovému poli, vložte hodnotu hello **formát názvu identifikátor** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0398b-183">In **Name ID Format** textbox, paste hello value of **Name Identifier Format** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="0398b-184">e.</span><span class="sxs-lookup"><span data-stu-id="0398b-184">e.</span></span> <span data-ttu-id="0398b-185">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0398b-185">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="0398b-186">f.</span><span class="sxs-lookup"><span data-stu-id="0398b-186">f.</span></span> <span data-ttu-id="0398b-187">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="0398b-187">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="0398b-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0398b-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0398b-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0398b-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0398b-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0398b-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0398b-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0398b-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="0398b-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0398b-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0398b-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0398b-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0398b-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0398b-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0398b-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0398b-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0398b-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="0398b-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0398b-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0398b-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0398b-203">a.</span><span class="sxs-lookup"><span data-stu-id="0398b-203">a.</span></span> <span data-ttu-id="0398b-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0398b-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0398b-205">b.</span><span class="sxs-lookup"><span data-stu-id="0398b-205">b.</span></span> <span data-ttu-id="0398b-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0398b-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0398b-207">c.</span><span class="sxs-lookup"><span data-stu-id="0398b-207">c.</span></span> <span data-ttu-id="0398b-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0398b-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0398b-209">d.</span><span class="sxs-lookup"><span data-stu-id="0398b-209">d.</span></span> <span data-ttu-id="0398b-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0398b-210">Click **Create**.</span></span>
 
### <a name="creating-a-gigya-test-user"></a><span data-ttu-id="0398b-211">Vytvoření zkušebního uživatele Gigya</span><span class="sxs-lookup"><span data-stu-id="0398b-211">Creating a Gigya test user</span></span>

<span data-ttu-id="0398b-212">V pořadí tooenable Azure AD Uživatelé toolog do Gigya musí být zřízená do Gigya.</span><span class="sxs-lookup"><span data-stu-id="0398b-212">In order tooenable Azure AD users toolog into Gigya, they must be provisioned into Gigya.</span></span>  
<span data-ttu-id="0398b-213">V případě hello Gigya zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="0398b-213">In hello case of Gigya, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="0398b-214">tooprovision uživatelské účty, provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0398b-214">tooprovision a user accounts, perform hello following steps:</span></span>

1. <span data-ttu-id="0398b-215">Přihlaste se tooyour **Gigya** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="0398b-215">Log in tooyour **Gigya** company site as an administrator.</span></span>

2. <span data-ttu-id="0398b-216">Přejděte příliš**správce \> spravovat uživatele**a potom klikněte na **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0398b-216">Go too**Admin \> Manage Users**, and then click **Invite Users**.</span></span>
   
    <span data-ttu-id="0398b-217">![Správa uživatelů](./media/active-directory-saas-gigya-tutorial/ic789535.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="0398b-217">![Manage Users](./media/active-directory-saas-gigya-tutorial/ic789535.png "Manage Users")</span></span>

3. <span data-ttu-id="0398b-218">V dialogovém okně hello pozvat uživatele proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="0398b-218">On hello Invite Users dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="0398b-219">![Uživatele pozvat](./media/active-directory-saas-gigya-tutorial/ic789536.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="0398b-219">![Invite Users](./media/active-directory-saas-gigya-tutorial/ic789536.png "Invite Users")</span></span>
   
    <span data-ttu-id="0398b-220">a.</span><span class="sxs-lookup"><span data-stu-id="0398b-220">a.</span></span> <span data-ttu-id="0398b-221">V hello **e-mailu** textovému poli, typ hello e-mailový alias chcete tooprovision platný účet služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0398b-221">In hello **Email** textbox, type hello email alias of a valid Azure Active Directory account you want tooprovision.</span></span>
    
    <span data-ttu-id="0398b-222">b.</span><span class="sxs-lookup"><span data-stu-id="0398b-222">b.</span></span> <span data-ttu-id="0398b-223">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0398b-223">Click **Invite User**.</span></span>
      
    > [!NOTE]
    > <span data-ttu-id="0398b-224">Držitel účtu Azure Active Directory Hello obdrží e-mail, který obsahuje účet odkaz tooconfirm hello předtím, než se stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="0398b-224">hello Azure Active Directory account holder will receive an email that includes a link tooconfirm hello account before it becomes active.</span></span>
    > 
    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0398b-225">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0398b-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0398b-226">V této části povolíte tak, že udělíte přístup tooGigya toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0398b-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGigya.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0398b-228">**tooassign Britta Simon tooGigya, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0398b-228">**tooassign Britta Simon tooGigya, perform hello following steps:**</span></span>

1. <span data-ttu-id="0398b-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0398b-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0398b-231">V seznamu aplikace hello vyberte **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="0398b-231">In hello applications list, select **Gigya**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. <span data-ttu-id="0398b-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0398b-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0398b-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0398b-235">Click **Add** button.</span></span> <span data-ttu-id="0398b-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0398b-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0398b-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0398b-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0398b-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0398b-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0398b-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0398b-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0398b-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0398b-241">Testing single sign-on</span></span>

<span data-ttu-id="0398b-242">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="0398b-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="0398b-243">Když kliknete na dlaždici Gigya hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Gigya aplikace.</span><span class="sxs-lookup"><span data-stu-id="0398b-243">When you click hello Gigya tile in hello Access Panel, you should get automatically signed-on tooyour Gigya application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0398b-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0398b-244">Additional resources</span></span>

* [<span data-ttu-id="0398b-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0398b-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0398b-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0398b-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

