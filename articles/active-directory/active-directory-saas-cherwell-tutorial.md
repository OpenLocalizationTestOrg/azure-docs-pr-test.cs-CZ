---
title: 'Kurz: Azure Active Directory integrace s Cherwell | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Cherwell."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: jeedes
ms.openlocfilehash: a67b3d346a6f7b43a7e87fb4d9c533f9363f2e02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a><span data-ttu-id="603ae-103">Kurz: Azure Active Directory integrace s Cherwell</span><span class="sxs-lookup"><span data-stu-id="603ae-103">Tutorial: Azure Active Directory integration with Cherwell</span></span>

<span data-ttu-id="603ae-104">V tomto kurzu zjistíte, jak toointegrate Cherwell s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="603ae-104">In this tutorial, you learn how toointegrate Cherwell with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="603ae-105">Integrace Cherwell s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="603ae-105">Integrating Cherwell with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="603ae-106">Můžete řídit ve službě Azure AD, který má přístup tooCherwell</span><span class="sxs-lookup"><span data-stu-id="603ae-106">You can control in Azure AD who has access tooCherwell</span></span>
- <span data-ttu-id="603ae-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCherwell (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="603ae-107">You can enable your users tooautomatically get signed-on tooCherwell (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="603ae-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="603ae-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="603ae-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="603ae-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="603ae-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="603ae-110">Prerequisites</span></span>

<span data-ttu-id="603ae-111">Integrace služby Azure AD s Cherwell tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="603ae-111">tooconfigure Azure AD integration with Cherwell, you need hello following items:</span></span>

- <span data-ttu-id="603ae-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="603ae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="603ae-113">Cherwell jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="603ae-113">A Cherwell single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="603ae-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="603ae-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="603ae-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="603ae-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="603ae-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="603ae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="603ae-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="603ae-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="603ae-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="603ae-118">Scenario description</span></span>
<span data-ttu-id="603ae-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="603ae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="603ae-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="603ae-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="603ae-121">Přidání Cherwell z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="603ae-121">Adding Cherwell from hello gallery</span></span>
2. <span data-ttu-id="603ae-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="603ae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cherwell-from-hello-gallery"></a><span data-ttu-id="603ae-123">Přidání Cherwell z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="603ae-123">Adding Cherwell from hello gallery</span></span>
<span data-ttu-id="603ae-124">tooconfigure hello integrace Cherwell do Azure AD, je nutné tooadd Cherwell hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="603ae-124">tooconfigure hello integration of Cherwell into Azure AD, you need tooadd Cherwell from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="603ae-125">**tooadd Cherwell z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="603ae-125">**tooadd Cherwell from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="603ae-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="603ae-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="603ae-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="603ae-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="603ae-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="603ae-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="603ae-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="603ae-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="603ae-133">Hello vyhledávacího pole zadejte **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="603ae-133">In hello search box, type **Cherwell**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_search.png)

5. <span data-ttu-id="603ae-135">Na panelu výsledků hello vyberte **Cherwell**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="603ae-135">In hello results panel, select **Cherwell**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="603ae-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="603ae-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="603ae-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Cherwell podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="603ae-138">In this section, you configure and test Azure AD single sign-on with Cherwell based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="603ae-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Cherwell je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="603ae-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cherwell is tooa user in Azure AD.</span></span> <span data-ttu-id="603ae-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Cherwell musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="603ae-140">In other words, a link relationship between an Azure AD user and hello related user in Cherwell needs toobe established.</span></span>

<span data-ttu-id="603ae-141">V Cherwell, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="603ae-141">In Cherwell, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="603ae-142">tooconfigure a testu Azure AD jednotné přihlašování s Cherwell, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="603ae-142">tooconfigure and test Azure AD single sign-on with Cherwell, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="603ae-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="603ae-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="603ae-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="603ae-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="603ae-145">**[Vytvoření zkušebního uživatele Cherwell](#creating-a-cherwell-test-user)**  -toohave protějšek Britta Simon v Cherwell, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="603ae-145">**[Creating a Cherwell test user](#creating-a-cherwell-test-user)** - toohave a counterpart of Britta Simon in Cherwell that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="603ae-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="603ae-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="603ae-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="603ae-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="603ae-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="603ae-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="603ae-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Cherwell.</span><span class="sxs-lookup"><span data-stu-id="603ae-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cherwell application.</span></span>

<span data-ttu-id="603ae-150">**tooconfigure Azure AD jednotné přihlašování s Cherwell, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="603ae-150">**tooconfigure Azure AD single sign-on with Cherwell, perform hello following steps:**</span></span>

1. <span data-ttu-id="603ae-151">V portálu Azure, na hello hello **Cherwell** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="603ae-151">In hello Azure portal, on hello **Cherwell** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="603ae-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="603ae-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_samlbase.png)

3. <span data-ttu-id="603ae-155">Na hello **Cherwell domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="603ae-155">On hello **Cherwell Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_url.png)

    <span data-ttu-id="603ae-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.cherwellondemand.com/cherwellclient`</span><span class="sxs-lookup"><span data-stu-id="603ae-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.cherwellondemand.com/cherwellclient`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="603ae-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="603ae-158">This value is not real.</span></span> <span data-ttu-id="603ae-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="603ae-159">Update this value with hello actual Sign-on URL.</span></span> <span data-ttu-id="603ae-160">Obraťte se na [tým podpory Cherwell](https://csm.cherwell.com/contact) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="603ae-160">Contact [Cherwell support team](https://csm.cherwell.com/contact) tooget this value.</span></span>
 
4. <span data-ttu-id="603ae-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="603ae-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_certificate.png) 

5. <span data-ttu-id="603ae-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="603ae-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="603ae-165">Na hello **Cherwell konfigurace** klikněte na tlačítko **konfigurace Cherwell** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="603ae-165">On hello **Cherwell Configuration** section, click **Configure Cherwell** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="603ae-166">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="603ae-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_configure.png) 

7. <span data-ttu-id="603ae-168">tooconfigure jednotného přihlašování na **Cherwell** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **SAML jeden přihlašování adresa URL služby**, a  **SAML Entity ID** příliš[tým podpory Cherwell](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="603ae-168">tooconfigure single sign-on on **Cherwell** side, you need toosend hello downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Cherwell support team](https://csm.cherwell.com/contact).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="603ae-169">Váš tým podpory Cherwell má toodo hello skutečné jednotné přihlašování konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="603ae-169">Your Cherwell support team has toodo hello actual SSO configuration.</span></span> <span data-ttu-id="603ae-170">Zobrazí se oznámení, když bylo povoleno jednotné přihlašování pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="603ae-170">You will get a notification when SSO has been enabled for your subscription.</span></span>
    > 
    
> [!TIP]
> <span data-ttu-id="603ae-171">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="603ae-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="603ae-172">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="603ae-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="603ae-173">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="603ae-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="603ae-174">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="603ae-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="603ae-175">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="603ae-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="603ae-177">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="603ae-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="603ae-178">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="603ae-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="603ae-180">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="603ae-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="603ae-182">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="603ae-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="603ae-184">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="603ae-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="603ae-186">a.</span><span class="sxs-lookup"><span data-stu-id="603ae-186">a.</span></span> <span data-ttu-id="603ae-187">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="603ae-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="603ae-188">b.</span><span class="sxs-lookup"><span data-stu-id="603ae-188">b.</span></span> <span data-ttu-id="603ae-189">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="603ae-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="603ae-190">c.</span><span class="sxs-lookup"><span data-stu-id="603ae-190">c.</span></span> <span data-ttu-id="603ae-191">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="603ae-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="603ae-192">d.</span><span class="sxs-lookup"><span data-stu-id="603ae-192">d.</span></span> <span data-ttu-id="603ae-193">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="603ae-193">Click **Create**.</span></span>
 
### <a name="creating-a-cherwell-test-user"></a><span data-ttu-id="603ae-194">Vytvoření zkušebního uživatele Cherwell</span><span class="sxs-lookup"><span data-stu-id="603ae-194">Creating a Cherwell test user</span></span>

<span data-ttu-id="603ae-195">Uživatelé toolog tooenable Azure AD v tooCherwell, se musí být zřízená do Cherwell.</span><span class="sxs-lookup"><span data-stu-id="603ae-195">tooenable Azure AD users toolog in tooCherwell, they must be provisioned into Cherwell.</span></span>

<span data-ttu-id="603ae-196">V případě hello Cherwell, hello uživatelské účty musí toobe vytvořené vaše [tým podpory Cherwell](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="603ae-196">In hello case of Cherwell, hello user accounts need toobe created by your [Cherwell support team](https://csm.cherwell.com/contact).</span></span>

>[!NOTE]
><span data-ttu-id="603ae-197">Můžete použít všechny ostatní Cherwell uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Cherwell tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="603ae-197">You can use any other Cherwell user account creation tools or APIs provided by Cherwell tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="603ae-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="603ae-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="603ae-199">V této části povolíte tak, že udělíte přístup tooCherwell toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="603ae-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCherwell.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="603ae-201">**tooassign Britta Simon tooCherwell, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="603ae-201">**tooassign Britta Simon tooCherwell, perform hello following steps:**</span></span>

1. <span data-ttu-id="603ae-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="603ae-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="603ae-204">V seznamu aplikace hello vyberte **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="603ae-204">In hello applications list, select **Cherwell**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_app.png) 

3. <span data-ttu-id="603ae-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="603ae-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="603ae-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="603ae-208">Click **Add** button.</span></span> <span data-ttu-id="603ae-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="603ae-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="603ae-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="603ae-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="603ae-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="603ae-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="603ae-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="603ae-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="603ae-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="603ae-214">Testing single sign-on</span></span>

<span data-ttu-id="603ae-215">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="603ae-215">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="603ae-216">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="603ae-216">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="603ae-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="603ae-217">Additional resources</span></span>

* [<span data-ttu-id="603ae-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="603ae-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="603ae-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="603ae-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_203.png

