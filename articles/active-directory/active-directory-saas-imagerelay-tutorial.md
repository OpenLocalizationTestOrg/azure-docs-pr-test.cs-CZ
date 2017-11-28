---
title: "Kurz: Azure Active Directory integrace s Image předávání | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a předávací bitové kopie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: baf39e4437b85c2de5b524984ad5ca39badbab63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="01def-103">Kurz: Azure Active Directory integrace s předávání bitové kopie</span><span class="sxs-lookup"><span data-stu-id="01def-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="01def-104">V tomto kurzu zjistíte, jak toointegrate předávání bitové kopie s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01def-104">In this tutorial, you learn how toointegrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="01def-105">Integrace předávání bitové kopie s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="01def-105">Integrating Image Relay with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="01def-106">Můžete řídit ve službě Azure AD, který má přístup tooImage předávání</span><span class="sxs-lookup"><span data-stu-id="01def-106">You can control in Azure AD who has access tooImage Relay</span></span>
- <span data-ttu-id="01def-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooImage předávání (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="01def-107">You can enable your users tooautomatically get signed-on tooImage Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="01def-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="01def-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="01def-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="01def-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01def-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01def-110">Prerequisites</span></span>

<span data-ttu-id="01def-111">tooconfigure integrace Azure AD s předávání bitové kopie, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="01def-111">tooconfigure Azure AD integration with Image Relay, you need hello following items:</span></span>

- <span data-ttu-id="01def-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01def-112">An Azure AD subscription</span></span>
- <span data-ttu-id="01def-113">Předávání Image jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="01def-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="01def-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="01def-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="01def-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="01def-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="01def-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="01def-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="01def-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="01def-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="01def-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="01def-118">Scenario description</span></span>
<span data-ttu-id="01def-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="01def-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="01def-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="01def-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="01def-121">Přidání bitové kopie předávání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="01def-121">Adding Image Relay from hello gallery</span></span>
2. <span data-ttu-id="01def-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01def-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-hello-gallery"></a><span data-ttu-id="01def-123">Přidání bitové kopie předávání z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="01def-123">Adding Image Relay from hello gallery</span></span>
<span data-ttu-id="01def-124">tooconfigure hello integrace předávání bitové kopie do Azure AD, je nutné tooadd předávání bitové kopie z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="01def-124">tooconfigure hello integration of Image Relay into Azure AD, you need tooadd Image Relay from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="01def-125">**tooadd předávání bitové kopie z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="01def-125">**tooadd Image Relay from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="01def-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01def-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="01def-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="01def-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="01def-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01def-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="01def-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01def-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="01def-133">Hello vyhledávacího pole zadejte **Image předávání**.</span><span class="sxs-lookup"><span data-stu-id="01def-133">In hello search box, type **Image Relay**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="01def-135">Na panelu výsledků hello vyberte **Image předávání**a pak klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="01def-135">In hello results panel, select **Image Relay**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="01def-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="01def-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="01def-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s předávání bitové kopie založené na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="01def-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="01def-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Relay bitové kopie je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01def-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Image Relay is tooa user in Azure AD.</span></span> <span data-ttu-id="01def-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Relay Image musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="01def-140">In other words, a link relationship between an Azure AD user and hello related user in Image Relay needs toobe established.</span></span>

<span data-ttu-id="01def-141">V předávání bitové kopie, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="01def-141">In Image Relay, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="01def-142">tooconfigure a testu Azure AD jednotné přihlašování s předávání bitové kopie, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="01def-142">tooconfigure and test Azure AD single sign-on with Image Relay, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="01def-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="01def-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="01def-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="01def-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="01def-145">**[Vytváření testovacího uživatele předávání Image](#creating-an-image-relay-test-user)**  -toohave protějšek Britta Simon Relay bitovou kopii, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="01def-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - toohave a counterpart of Britta Simon in Image Relay that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="01def-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01def-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="01def-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="01def-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="01def-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01def-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="01def-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="01def-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="01def-150">**tooconfigure Azure AD jednotné přihlašování s předávání bitové kopie, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="01def-150">**tooconfigure Azure AD single sign-on with Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="01def-151">V portálu Azure, na hello hello **Image předávání** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="01def-151">In hello Azure portal, on hello **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="01def-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01def-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="01def-155">Na hello **Image předávání domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="01def-155">On hello **Image Relay Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="01def-157">a.</span><span class="sxs-lookup"><span data-stu-id="01def-157">a.</span></span> <span data-ttu-id="01def-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="01def-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="01def-159">b.</span><span class="sxs-lookup"><span data-stu-id="01def-159">b.</span></span> <span data-ttu-id="01def-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="01def-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="01def-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="01def-161">These values are not real.</span></span> <span data-ttu-id="01def-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="01def-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="01def-163">Obraťte se na [tým podpory Image předávání klienta](http://support.imagerelay.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="01def-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="01def-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="01def-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="01def-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01def-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="01def-168">Na hello **konfigurace přenosového Image** klikněte na tlačítko **konfigurace přenosového bitové kopie** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="01def-168">On hello **Image Relay Configuration** section, click **Configure Image Relay** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="01def-169">Kopírování hello **Sign-Out adresa URL služby a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="01def-169">Copy hello **Sign-Out Service URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="01def-171">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="01def-171">In another browser window, sign in tooyour Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="01def-172">V panelu nástrojů hello hello nahoře, klikněte na tlačítko hello **uživatelé a oprávnění** zatížení.</span><span class="sxs-lookup"><span data-stu-id="01def-172">In hello toolbar on hello top, click hello **Users & Permissions** workload.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="01def-174">Klikněte na tlačítko **vytvořit nová položka oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="01def-174">Click **Create New Permission**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="01def-176">V hello **jeden znak v nastavení** úlohy, vyberte hello **tato skupina může pouze přihlášení přes jednotné přihlašování** zaškrtněte políčko a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="01def-176">In hello **Single Sign On Settings** workload, select hello **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="01def-178">Přejděte příliš**nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="01def-178">Go too**Account Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="01def-180">Přejděte toohello **jeden znak v nastavení** zatížení.</span><span class="sxs-lookup"><span data-stu-id="01def-180">Go toohello **Single Sign On Settings** workload.</span></span>
    
     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="01def-182">Na hello **SAML nastavení** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="01def-182">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="01def-184">a.</span><span class="sxs-lookup"><span data-stu-id="01def-184">a.</span></span> <span data-ttu-id="01def-185">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="01def-185">In **Login URL** textbox, paste hello value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="01def-186">b.</span><span class="sxs-lookup"><span data-stu-id="01def-186">b.</span></span> <span data-ttu-id="01def-187">V **adresy URL odhlašovací** textovému poli, vložte hodnotu hello **jednu adresu URL služby Sign-Out** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="01def-187">In **Logout URL**  textbox, paste hello value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="01def-188">c.</span><span class="sxs-lookup"><span data-stu-id="01def-188">c.</span></span> <span data-ttu-id="01def-189">Jako **formát Id názvu**, vyberte **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="01def-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="01def-190">d.</span><span class="sxs-lookup"><span data-stu-id="01def-190">d.</span></span> <span data-ttu-id="01def-191">Jako **vazby možnosti pro žádosti od hello poskytovatele služeb (Image relé)**, vyberte **POST vazby**.</span><span class="sxs-lookup"><span data-stu-id="01def-191">As **Binding Options for Requests from hello Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="01def-192">e.</span><span class="sxs-lookup"><span data-stu-id="01def-192">e.</span></span> <span data-ttu-id="01def-193">V části **x.509 Certificate**, klikněte na tlačítko **aktualizace certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="01def-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="01def-195">f.</span><span class="sxs-lookup"><span data-stu-id="01def-195">f.</span></span> <span data-ttu-id="01def-196">V poznámkovém bloku otevřete hello stáhnout certifikát, zkopírujte hello obsah a pak ji vložit do textového pole certifikátu x.509 hello.</span><span class="sxs-lookup"><span data-stu-id="01def-196">Open hello downloaded certificate in notepad, copy hello content, and then paste it into hello x.509 Certificate textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="01def-198">g.</span><span class="sxs-lookup"><span data-stu-id="01def-198">g.</span></span> <span data-ttu-id="01def-199">V **zřizování uživatelů JIT** části, vyberte hello **povolit zřizování uživatelů JIT**.</span><span class="sxs-lookup"><span data-stu-id="01def-199">In **Just-In-Time User Provisioning** section, select hello **Enable Just-In-Time User Provisioning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="01def-201">h.</span><span class="sxs-lookup"><span data-stu-id="01def-201">h.</span></span> <span data-ttu-id="01def-202">Vyberte hello skupině oprávnění (například **jednotného přihlašování k základní**) povolený toosign v pouze prostřednictvím jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01def-202">Select hello permission group (for example, **SSO Basic**) which is allowed toosign in only through single sign-on.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="01def-204">i.</span><span class="sxs-lookup"><span data-stu-id="01def-204">i.</span></span> <span data-ttu-id="01def-205">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="01def-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="01def-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="01def-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="01def-207">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="01def-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="01def-208">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="01def-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="01def-209">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="01def-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="01def-210">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="01def-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="01def-212">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="01def-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="01def-213">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="01def-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="01def-215">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="01def-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="01def-217">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="01def-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="01def-219">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="01def-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="01def-221">a.</span><span class="sxs-lookup"><span data-stu-id="01def-221">a.</span></span> <span data-ttu-id="01def-222">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="01def-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="01def-223">b.</span><span class="sxs-lookup"><span data-stu-id="01def-223">b.</span></span> <span data-ttu-id="01def-224">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="01def-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="01def-225">c.</span><span class="sxs-lookup"><span data-stu-id="01def-225">c.</span></span> <span data-ttu-id="01def-226">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="01def-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="01def-227">d.</span><span class="sxs-lookup"><span data-stu-id="01def-227">d.</span></span> <span data-ttu-id="01def-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01def-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="01def-229">Vytváření testovacího uživatele předávání bitové kopie</span><span class="sxs-lookup"><span data-stu-id="01def-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="01def-230">Hello cílem této části je toocreate uživatel volal Britta Simon v předávání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="01def-230">hello objective of this section is toocreate a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="01def-231">**toocreate uživatel volal Britta Simon v předávání bitové kopie, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="01def-231">**toocreate a user called Britta Simon in Image Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="01def-232">Web společnosti předávání bitové kopie tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="01def-232">Sign-on tooyour Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="01def-233">Přejděte příliš**uživatelé a oprávnění** a vyberte **vytvořit jednotné přihlašování uživatele**.</span><span class="sxs-lookup"><span data-stu-id="01def-233">Go too**Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="01def-235">Zadejte hello **e-mailu**, **křestní jméno**, **příjmení**, a **společnosti** hello uživatele chcete tooprovision a vyberte hello oprávnění skupiny (například jednotného přihlašování k základní) tedy hello skupinu, která může přihlásit pouze pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01def-235">Enter hello **Email**, **First Name**, **Last Name**, and **Company** of hello user you want tooprovision and select hello permission group (for example, SSO Basic) which is hello group that can sign in only through single sign-on.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="01def-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01def-237">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="01def-238">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="01def-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="01def-239">V této části povolíte tak, že udělíte přístup tooImage předávání Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="01def-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooImage Relay.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="01def-241">**tooassign tooImage Britta Simon předávání, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="01def-241">**tooassign Britta Simon tooImage Relay, perform hello following steps:**</span></span>

1. <span data-ttu-id="01def-242">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01def-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="01def-244">V seznamu aplikace hello vyberte **Image předávání**.</span><span class="sxs-lookup"><span data-stu-id="01def-244">In hello applications list, select **Image Relay**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="01def-246">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="01def-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="01def-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="01def-248">Click **Add** button.</span></span> <span data-ttu-id="01def-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01def-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="01def-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="01def-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="01def-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01def-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="01def-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="01def-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="01def-254">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="01def-254">Testing single sign-on</span></span>

<span data-ttu-id="01def-255">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="01def-255">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>    

<span data-ttu-id="01def-256">Když kliknete na dlaždici hello předávání bitové kopie v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour předávání Image aplikace.</span><span class="sxs-lookup"><span data-stu-id="01def-256">When you click hello Image Relay tile in hello Access Panel, you should get automatically signed-on tooyour Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="01def-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="01def-257">Additional resources</span></span>

* [<span data-ttu-id="01def-258">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="01def-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="01def-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="01def-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

