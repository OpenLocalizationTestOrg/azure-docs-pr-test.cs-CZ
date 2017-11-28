---
title: 'Kurz: Azure Active Directory integrace s Panopto | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Panopto."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 76b30e1cd2782bb5fba3d229378b8f82652b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a><span data-ttu-id="5272c-103">Kurz: Azure Active Directory integrace s Panopto</span><span class="sxs-lookup"><span data-stu-id="5272c-103">Tutorial: Azure Active Directory integration with Panopto</span></span>

<span data-ttu-id="5272c-104">V tomto kurzu zjistíte, jak toointegrate Panopto s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5272c-104">In this tutorial, you learn how toointegrate Panopto with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5272c-105">Integrace Panopto s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5272c-105">Integrating Panopto with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5272c-106">Můžete řídit ve službě Azure AD, který má přístup tooPanopto</span><span class="sxs-lookup"><span data-stu-id="5272c-106">You can control in Azure AD who has access tooPanopto</span></span>
- <span data-ttu-id="5272c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPanopto (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5272c-107">You can enable your users tooautomatically get signed-on tooPanopto (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5272c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5272c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5272c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5272c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5272c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5272c-110">Prerequisites</span></span>

<span data-ttu-id="5272c-111">Integrace služby Azure AD s Panopto tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5272c-111">tooconfigure Azure AD integration with Panopto, you need hello following items:</span></span>

- <span data-ttu-id="5272c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5272c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5272c-113">Panopto jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5272c-113">A Panopto single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5272c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5272c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5272c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5272c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5272c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5272c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5272c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5272c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5272c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5272c-118">Scenario description</span></span>
<span data-ttu-id="5272c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5272c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5272c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5272c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5272c-121">Přidání Panopto z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5272c-121">Adding Panopto from hello gallery</span></span>
2. <span data-ttu-id="5272c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5272c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panopto-from-hello-gallery"></a><span data-ttu-id="5272c-123">Přidání Panopto z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5272c-123">Adding Panopto from hello gallery</span></span>
<span data-ttu-id="5272c-124">tooconfigure hello integrace Panopto do Azure AD, je nutné tooadd Panopto hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5272c-124">tooconfigure hello integration of Panopto into Azure AD, you need tooadd Panopto from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5272c-125">**tooadd Panopto z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5272c-125">**tooadd Panopto from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5272c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5272c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5272c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5272c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5272c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5272c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5272c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5272c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5272c-133">Hello vyhledávacího pole zadejte **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="5272c-133">In hello search box, type **Panopto**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_search.png)

5. <span data-ttu-id="5272c-135">Na panelu výsledků hello vyberte **Panopto**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5272c-135">In hello results panel, select **Panopto**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5272c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5272c-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="5272c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panopto podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5272c-138">In this section, you configure and test Azure AD single sign-on with Panopto based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5272c-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Panopto je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5272c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Panopto is tooa user in Azure AD.</span></span> <span data-ttu-id="5272c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Panopto musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5272c-140">In other words, a link relationship between an Azure AD user and hello related user in Panopto needs toobe established.</span></span>

<span data-ttu-id="5272c-141">V Panopto, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5272c-141">In Panopto, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5272c-142">tooconfigure a testu Azure AD jednotné přihlašování s Panopto, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5272c-142">tooconfigure and test Azure AD single sign-on with Panopto, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5272c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5272c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5272c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5272c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5272c-145">**[Vytvoření zkušebního uživatele Panopto](#creating-a-panopto-test-user)**  -toohave protějšek Britta Simon v Panopto, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5272c-145">**[Creating a Panopto test user](#creating-a-panopto-test-user)** - toohave a counterpart of Britta Simon in Panopto that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5272c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5272c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5272c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5272c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5272c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5272c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5272c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Panopto.</span><span class="sxs-lookup"><span data-stu-id="5272c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Panopto application.</span></span>

<span data-ttu-id="5272c-150">**tooconfigure Azure AD jednotné přihlašování s Panopto, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5272c-150">**tooconfigure Azure AD single sign-on with Panopto, perform hello following steps:**</span></span>

1. <span data-ttu-id="5272c-151">V portálu Azure, na hello hello **Panopto** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5272c-151">In hello Azure portal, on hello **Panopto** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5272c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5272c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_samlbase.png)

3. <span data-ttu-id="5272c-155">Na hello **Panopto domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5272c-155">On hello **Panopto Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_url.png)

    <span data-ttu-id="5272c-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenant-name>.panopto.com`</span><span class="sxs-lookup"><span data-stu-id="5272c-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.panopto.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5272c-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="5272c-158">This value is not real.</span></span> <span data-ttu-id="5272c-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5272c-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="5272c-160">Obraťte se na [tým podpory Panopto klienta](mailto:support@panopto.com‎) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5272c-160">Contact [Panopto Client support team](mailto:support@panopto.com‎) tooget this value.</span></span> 
 
4. <span data-ttu-id="5272c-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5272c-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_certificate.png) 

5. <span data-ttu-id="5272c-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5272c-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5272c-165">Na hello **Panopto konfigurace** klikněte na tlačítko **konfigurace Panopto** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5272c-165">On hello **Panopto Configuration** section, click **Configure Panopto** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5272c-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5272c-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_configure.png) 

7. <span data-ttu-id="5272c-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Panopto tooyour.</span><span class="sxs-lookup"><span data-stu-id="5272c-168">In a different web browser window, log in tooyour Panopto company site as an administrator.</span></span>

8. <span data-ttu-id="5272c-169">V panelu nástrojů hello na levé straně hello, klikněte na **systému**a potom klikněte na **zprostředkovatelů Identity**.</span><span class="sxs-lookup"><span data-stu-id="5272c-169">In hello toolbar on hello left, click **System**, and then click **Identity Providers**.</span></span>
   
   <span data-ttu-id="5272c-170">![Systém](./media/active-directory-saas-panopto-tutorial/ic777670.png "systému")</span><span class="sxs-lookup"><span data-stu-id="5272c-170">![System](./media/active-directory-saas-panopto-tutorial/ic777670.png "System")</span></span>
9. <span data-ttu-id="5272c-171">Klikněte na tlačítko **přidejte zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="5272c-171">Click **Add Provider**.</span></span>
   
   <span data-ttu-id="5272c-172">![Zprostředkovatelé identity](./media/active-directory-saas-panopto-tutorial/ic777671.png "poskytovatelů identit")</span><span class="sxs-lookup"><span data-stu-id="5272c-172">![Identity Providers](./media/active-directory-saas-panopto-tutorial/ic777671.png "Identity Providers")</span></span>
   
10. <span data-ttu-id="5272c-173">V části zprostředkovatele SAML hello proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5272c-173">In hello SAML provider section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5272c-174">![Konfigurace SaaS](./media/active-directory-saas-panopto-tutorial/ic777672.png "konfigurace SaaS")</span><span class="sxs-lookup"><span data-stu-id="5272c-174">![SaaS configuration](./media/active-directory-saas-panopto-tutorial/ic777672.png "SaaS configuration")</span></span>
    
    <span data-ttu-id="5272c-175">a.</span><span class="sxs-lookup"><span data-stu-id="5272c-175">a.</span></span> <span data-ttu-id="5272c-176">Z hello **typ zprostředkovatele** seznamu, vyberte **SAML20**.</span><span class="sxs-lookup"><span data-stu-id="5272c-176">From hello **Provider Type** list, select **SAML20**.</span></span>    
    
    <span data-ttu-id="5272c-177">b.</span><span class="sxs-lookup"><span data-stu-id="5272c-177">b.</span></span> <span data-ttu-id="5272c-178">V hello **název Instance** textovému poli, zadejte název pro instanci hello.</span><span class="sxs-lookup"><span data-stu-id="5272c-178">In hello **Instance Name** textbox, type a name for hello instance.</span></span>

    <span data-ttu-id="5272c-179">c.</span><span class="sxs-lookup"><span data-stu-id="5272c-179">c.</span></span> <span data-ttu-id="5272c-180">V hello **stručný popis** textovému poli, zadejte popisný název.</span><span class="sxs-lookup"><span data-stu-id="5272c-180">In hello **Friendly Description** textbox, type a friendly description.</span></span>
    
    <span data-ttu-id="5272c-181">d.</span><span class="sxs-lookup"><span data-stu-id="5272c-181">d.</span></span> <span data-ttu-id="5272c-182">V **kolísají adresa Url stránky** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5272c-182">In **Bounce Page Url** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5272c-183">e.</span><span class="sxs-lookup"><span data-stu-id="5272c-183">e.</span></span> <span data-ttu-id="5272c-184">V hello **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5272c-184">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5272c-185">f.</span><span class="sxs-lookup"><span data-stu-id="5272c-185">f.</span></span> <span data-ttu-id="5272c-186">Otevření kódování base-64 kódovaného certifikátu, který jste si stáhli z Azure portálu kopie hello obsahu ho do schránky tooyour, a pak ji vložit toohello **veřejný klíč** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5272c-186">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy hello content of it in tooyour clipboard, and then paste it toohello **Public Key**  textbox.</span></span>

11. <span data-ttu-id="5272c-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5272c-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="5272c-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5272c-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5272c-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5272c-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5272c-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5272c-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5272c-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5272c-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="5272c-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5272c-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5272c-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5272c-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5272c-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5272c-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5272c-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5272c-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5272c-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5272c-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5272c-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5272c-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5272c-203">a.</span><span class="sxs-lookup"><span data-stu-id="5272c-203">a.</span></span> <span data-ttu-id="5272c-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5272c-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5272c-205">b.</span><span class="sxs-lookup"><span data-stu-id="5272c-205">b.</span></span> <span data-ttu-id="5272c-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5272c-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5272c-207">c.</span><span class="sxs-lookup"><span data-stu-id="5272c-207">c.</span></span> <span data-ttu-id="5272c-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5272c-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5272c-209">d.</span><span class="sxs-lookup"><span data-stu-id="5272c-209">d.</span></span> <span data-ttu-id="5272c-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5272c-210">Click **Create**.</span></span>
 
### <a name="creating-a-panopto-test-user"></a><span data-ttu-id="5272c-211">Vytvoření zkušebního uživatele Panopto</span><span class="sxs-lookup"><span data-stu-id="5272c-211">Creating a Panopto test user</span></span>

<span data-ttu-id="5272c-212">Neexistuje žádná položka akce, pro které uživatele tooconfigure zřizování tooPanopto.</span><span class="sxs-lookup"><span data-stu-id="5272c-212">There is no action item for you tooconfigure user provisioning tooPanopto.</span></span>  
<span data-ttu-id="5272c-213">Když se uživatel s přiřazenou pokusí toolog v tooPanopto pomocí hello přístupového panelu, Panopto ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="5272c-213">When an assigned user tries toolog in tooPanopto using hello access panel, Panopto checks whether hello user exists.</span></span>  

<span data-ttu-id="5272c-214">Pokud neexistuje žádný účet k dispozici dosud, je vytvářena automaticky nástrojem Panopto.</span><span class="sxs-lookup"><span data-stu-id="5272c-214">If there is no user account available yet, it is automatically created by Panopto.</span></span>

>[!NOTE]
><span data-ttu-id="5272c-215">Můžete použít všechny ostatní Panopto uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Panopto tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5272c-215">You can use any other Panopto user account creation tools or APIs provided by Panopto tooprovision Azure AD user accounts.</span></span>
>
>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5272c-216">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5272c-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5272c-217">V této části povolíte tak, že udělíte přístup tooPanopto toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5272c-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPanopto.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5272c-219">**tooassign Britta Simon tooPanopto, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5272c-219">**tooassign Britta Simon tooPanopto, perform hello following steps:**</span></span>

1. <span data-ttu-id="5272c-220">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5272c-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5272c-222">V seznamu aplikace hello vyberte **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="5272c-222">In hello applications list, select **Panopto**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_app.png) 

3. <span data-ttu-id="5272c-224">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5272c-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5272c-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5272c-226">Click **Add** button.</span></span> <span data-ttu-id="5272c-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5272c-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5272c-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5272c-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5272c-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5272c-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5272c-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5272c-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5272c-232">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5272c-232">Testing single sign-on</span></span>

<span data-ttu-id="5272c-233">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5272c-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5272c-234">Když kliknete na dlaždici Panopto hello v hello přístupového panelu, měli byste obdržet automaticky přihlašovací stránku Panopto aplikace.</span><span class="sxs-lookup"><span data-stu-id="5272c-234">When you click hello Panopto tile in hello Access Panel, you should get automatically login page of Panopto application.</span></span>
<span data-ttu-id="5272c-235">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5272c-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5272c-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5272c-236">Additional resources</span></span>

* [<span data-ttu-id="5272c-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5272c-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5272c-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5272c-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_203.png

