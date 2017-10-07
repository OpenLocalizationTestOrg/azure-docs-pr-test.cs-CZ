---
title: "Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a správy Portfolia certifikační Autority."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 571130f3be0529c986aa0d8a08e4172015cd0b40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="70528-103">Kurz: Azure Active Directory integrace s správy Portfolia certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="70528-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="70528-104">V tomto kurzu zjistíte, jak toointegrate správy Portfolia certifikační Autority se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="70528-104">In this tutorial, you learn how toointegrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70528-105">Integrace správy Portfolia certifikační Autority s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="70528-105">Integrating CA PPM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="70528-106">Můžete řídit ve službě Azure AD, který má přístup tooCA stran za MINUTU</span><span class="sxs-lookup"><span data-stu-id="70528-106">You can control in Azure AD who has access tooCA PPM</span></span>
- <span data-ttu-id="70528-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCA stran za MINUTU (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="70528-107">You can enable your users tooautomatically get signed-on tooCA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70528-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="70528-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="70528-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70528-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70528-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70528-110">Prerequisites</span></span>

<span data-ttu-id="70528-111">tooconfigure integrace Azure AD s správy Portfolia certifikační Autority, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="70528-111">tooconfigure Azure AD integration with CA PPM, you need hello following items:</span></span>

- <span data-ttu-id="70528-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="70528-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70528-113">Certifikační Autoritu správy Portfolia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="70528-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70528-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="70528-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70528-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="70528-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70528-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="70528-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70528-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70528-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70528-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="70528-118">Scenario description</span></span>
<span data-ttu-id="70528-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="70528-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70528-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="70528-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70528-121">Přidání správy Portfolia certifikační Autority z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70528-121">Adding CA PPM from hello gallery</span></span>
2. <span data-ttu-id="70528-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70528-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-hello-gallery"></a><span data-ttu-id="70528-123">Přidání správy Portfolia certifikační Autority z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70528-123">Adding CA PPM from hello gallery</span></span>
<span data-ttu-id="70528-124">tooconfigure hello integraci správy Portfolia certifikační Autority do služby Azure AD, je nutné tooadd certifikační Autority správy Portfolia hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="70528-124">tooconfigure hello integration of CA PPM into Azure AD, you need tooadd CA PPM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="70528-125">**tooadd správy Portfolia certifikační Autority z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70528-125">**tooadd CA PPM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="70528-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70528-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70528-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="70528-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="70528-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70528-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="70528-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70528-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="70528-133">Hello vyhledávacího pole zadejte **správy Portfolia certifikační Autority**.</span><span class="sxs-lookup"><span data-stu-id="70528-133">In hello search box, type **CA PPM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="70528-135">Na panelu výsledků hello vyberte **správy Portfolia certifikační Autority**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70528-135">In hello results panel, select **CA PPM**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70528-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70528-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70528-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s správy Portfolia certifikační Autority podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="70528-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="70528-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v správy Portfolia certifikační Autority je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70528-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CA PPM is tooa user in Azure AD.</span></span> <span data-ttu-id="70528-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v správy Portfolia certifikační Autority musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="70528-140">In other words, a link relationship between an Azure AD user and hello related user in CA PPM needs toobe established.</span></span>

<span data-ttu-id="70528-141">V CA správy Portfolia, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="70528-141">In CA PPM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="70528-142">tooconfigure a testu Azure AD jednotné přihlašování s správy Portfolia certifikační Autority, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="70528-142">tooconfigure and test Azure AD single sign-on with CA PPM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="70528-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="70528-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="70528-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70528-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70528-145">**[Vytvoření zkušebního uživatele správy Portfolia certifikační Autority](#creating-a-ca-ppm-test-user)**  -toohave protějšek Britta Simon v správy Portfolia certifikační Autority, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="70528-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - toohave a counterpart of Britta Simon in CA PPM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="70528-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70528-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70528-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="70528-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70528-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70528-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70528-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="70528-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="70528-150">**tooconfigure Azure AD jednotné přihlašování s certifikační Autority správy Portfolia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70528-150">**tooconfigure Azure AD single sign-on with CA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="70528-151">V portálu Azure, na hello hello **správy Portfolia certifikační Autority** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="70528-151">In hello Azure portal, on hello **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="70528-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70528-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="70528-155">Na hello **certifikační Autority správy Portfolia domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="70528-155">On hello **CA PPM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="70528-157">a.</span><span class="sxs-lookup"><span data-stu-id="70528-157">a.</span></span> <span data-ttu-id="70528-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="70528-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="70528-159">b.</span><span class="sxs-lookup"><span data-stu-id="70528-159">b.</span></span> <span data-ttu-id="70528-160">V hello **adresa URL odpovědi** textovému poli, typ jako:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="70528-160">In hello **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="70528-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="70528-161">This value is not real.</span></span> <span data-ttu-id="70528-162">Aktualizujte tuto hodnotu s hello skutečné identifikátor.</span><span class="sxs-lookup"><span data-stu-id="70528-162">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="70528-163">Obraťte se na [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="70528-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) tooget this value.</span></span>
 
4. <span data-ttu-id="70528-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="70528-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="70528-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70528-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70528-168">Na hello **konfiguraci certifikační Autority správy Portfolia** klikněte na tlačítko **konfigurace certifikační Autority správy Portfolia** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="70528-168">On hello **CA PPM Configuration** section, click **Configure CA PPM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="70528-169">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="70528-169">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="70528-171">tooconfigure jednotného přihlašování na **správy Portfolia certifikační Autority** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **SAML Entity ID** příliš[tým podpory správy Portfolia certifikační Autority ](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="70528-171">tooconfigure single sign-on on **CA PPM** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Entity ID** too[CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="70528-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="70528-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="70528-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="70528-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="70528-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70528-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70528-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="70528-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="70528-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="70528-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="70528-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70528-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="70528-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70528-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70528-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="70528-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70528-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="70528-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70528-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="70528-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70528-187">a.</span><span class="sxs-lookup"><span data-stu-id="70528-187">a.</span></span> <span data-ttu-id="70528-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70528-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70528-189">b.</span><span class="sxs-lookup"><span data-stu-id="70528-189">b.</span></span> <span data-ttu-id="70528-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="70528-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="70528-191">c.</span><span class="sxs-lookup"><span data-stu-id="70528-191">c.</span></span> <span data-ttu-id="70528-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="70528-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="70528-193">d.</span><span class="sxs-lookup"><span data-stu-id="70528-193">d.</span></span> <span data-ttu-id="70528-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="70528-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="70528-195">Vytvoření zkušebního uživatele správy Portfolia certifikační Autority</span><span class="sxs-lookup"><span data-stu-id="70528-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="70528-196">V této části vytvoříte uživatele volal Britta Simon v správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="70528-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="70528-197">Práce s [tým podpory správy Portfolia certifikační Autority](mailto:catechnicalsupport@ca.com) tooadd hello uživatelé v platformě hello správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="70528-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) tooadd hello users in hello CA PPM platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="70528-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="70528-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="70528-199">V této části povolíte tak, že udělíte přístup tooCA správy Portfolia Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70528-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCA PPM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="70528-201">**tooassign tooCA Britta Simon správy Portfolia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70528-201">**tooassign Britta Simon tooCA PPM, perform hello following steps:**</span></span>

1. <span data-ttu-id="70528-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70528-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="70528-204">V seznamu aplikace hello vyberte **správy Portfolia certifikační Autority**.</span><span class="sxs-lookup"><span data-stu-id="70528-204">In hello applications list, select **CA PPM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="70528-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="70528-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="70528-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70528-208">Click **Add** button.</span></span> <span data-ttu-id="70528-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70528-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="70528-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="70528-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="70528-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70528-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70528-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70528-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70528-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70528-214">Testing single sign-on</span></span>

<span data-ttu-id="70528-215">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="70528-215">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="70528-216">Když kliknete na dlaždici správy Portfolia certifikační Autority hello v hello přístupového panelu, měli byste obdržet aplikace automaticky přihlášeného tooyour správy Portfolia certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="70528-216">When you click hello CA PPM tile in hello Access Panel, you should get automatically signed-on tooyour CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70528-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="70528-217">Additional resources</span></span>

* [<span data-ttu-id="70528-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="70528-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70528-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70528-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

