---
title: 'Kurz: Azure Active Directory integrace s AppBlade | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a AppBlade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 06f3d8fcee97945c867bca6f3aebe15ecef04617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="13486-103">Kurz: Azure Active Directory integrace s AppBlade</span><span class="sxs-lookup"><span data-stu-id="13486-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="13486-104">V tomto kurzu zjistíte, jak toointegrate AppBlade s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="13486-104">In this tutorial, you learn how toointegrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="13486-105">Integrace AppBlade s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="13486-105">Integrating AppBlade with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="13486-106">Můžete řídit ve službě Azure AD, který má přístup tooAppBlade</span><span class="sxs-lookup"><span data-stu-id="13486-106">You can control in Azure AD who has access tooAppBlade</span></span>
- <span data-ttu-id="13486-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAppBlade (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="13486-107">You can enable your users tooautomatically get signed-on tooAppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="13486-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="13486-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="13486-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="13486-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13486-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="13486-110">Prerequisites</span></span>

<span data-ttu-id="13486-111">Integrace služby Azure AD s AppBlade tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="13486-111">tooconfigure Azure AD integration with AppBlade, you need hello following items:</span></span>

- <span data-ttu-id="13486-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="13486-112">An Azure AD subscription</span></span>
- <span data-ttu-id="13486-113">AppBlade jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="13486-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="13486-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="13486-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="13486-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="13486-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="13486-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="13486-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="13486-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="13486-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="13486-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="13486-118">Scenario description</span></span>
<span data-ttu-id="13486-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="13486-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="13486-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="13486-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="13486-121">Přidání AppBlade z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="13486-121">Adding AppBlade from hello gallery</span></span>
2. <span data-ttu-id="13486-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="13486-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-hello-gallery"></a><span data-ttu-id="13486-123">Přidání AppBlade z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="13486-123">Adding AppBlade from hello gallery</span></span>
<span data-ttu-id="13486-124">tooconfigure hello integrace AppBlade do Azure AD, je nutné tooadd AppBlade hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="13486-124">tooconfigure hello integration of AppBlade into Azure AD, you need tooadd AppBlade from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="13486-125">**tooadd AppBlade z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13486-125">**tooadd AppBlade from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="13486-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="13486-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="13486-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="13486-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="13486-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="13486-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="13486-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13486-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="13486-133">Hello vyhledávacího pole zadejte **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="13486-133">In hello search box, type **AppBlade**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="13486-135">Na panelu výsledků hello vyberte **AppBlade**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="13486-135">In hello results panel, select **AppBlade**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="13486-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="13486-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="13486-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s AppBlade podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="13486-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="13486-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v AppBlade je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="13486-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppBlade is tooa user in Azure AD.</span></span> <span data-ttu-id="13486-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v AppBlade musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="13486-140">In other words, a link relationship between an Azure AD user and hello related user in AppBlade needs toobe established.</span></span>

<span data-ttu-id="13486-141">V AppBlade, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="13486-141">In AppBlade, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="13486-142">tooconfigure a testu Azure AD jednotné přihlašování s AppBlade, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="13486-142">tooconfigure and test Azure AD single sign-on with AppBlade, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="13486-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="13486-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="13486-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="13486-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="13486-145">**[Vytváření testovacího uživatele AppBlade](#creating-an-appblade-test-user)**  -toohave protějšek Britta Simon v AppBlade, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="13486-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - toohave a counterpart of Britta Simon in AppBlade that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="13486-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13486-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="13486-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="13486-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="13486-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="13486-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="13486-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci AppBlade.</span><span class="sxs-lookup"><span data-stu-id="13486-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="13486-150">**tooconfigure Azure AD jednotné přihlašování s AppBlade, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13486-150">**tooconfigure Azure AD single sign-on with AppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="13486-151">V portálu Azure, na hello hello **AppBlade** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="13486-151">In hello Azure portal, on hello **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="13486-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13486-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="13486-155">Na hello **AppBlade domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="13486-155">On hello **AppBlade Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="13486-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="13486-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="13486-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="13486-158">This value is not real.</span></span> <span data-ttu-id="13486-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13486-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="13486-160">Obraťte se na [tým podpory AppBlade klienta](mailto:support@appblade.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="13486-160">Contact [AppBlade Client support team](mailto:support@appblade.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="13486-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="13486-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="13486-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13486-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="13486-165">tooconfigure jednotného přihlašování na **AppBlade** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory AppBlade](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="13486-165">tooconfigure single sign-on on **AppBlade** side, you need toosend hello downloaded **Metadata XML** too[AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="13486-166">Také, požádejte je tooconfigure hello **URL vystavitele jednotného přihlašování k** jako `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="13486-166">Also, please ask them tooconfigure hello **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="13486-167">Toto nastavení je povinné pro toowork přihlášení.</span><span class="sxs-lookup"><span data-stu-id="13486-167">This setting is required for single sign-on toowork.</span></span>


> [!TIP]
> <span data-ttu-id="13486-168">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="13486-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="13486-169">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="13486-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="13486-170">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="13486-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="13486-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="13486-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="13486-172">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="13486-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="13486-174">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13486-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="13486-175">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="13486-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="13486-177">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="13486-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="13486-179">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="13486-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="13486-181">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="13486-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="13486-183">a.</span><span class="sxs-lookup"><span data-stu-id="13486-183">a.</span></span> <span data-ttu-id="13486-184">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="13486-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="13486-185">b.</span><span class="sxs-lookup"><span data-stu-id="13486-185">b.</span></span> <span data-ttu-id="13486-186">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="13486-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="13486-187">c.</span><span class="sxs-lookup"><span data-stu-id="13486-187">c.</span></span> <span data-ttu-id="13486-188">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="13486-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="13486-189">d.</span><span class="sxs-lookup"><span data-stu-id="13486-189">d.</span></span> <span data-ttu-id="13486-190">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="13486-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="13486-191">Vytváření testovacího uživatele AppBlade</span><span class="sxs-lookup"><span data-stu-id="13486-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="13486-192">Hello cílem této části je toocreate volal Britta Simon v AppBlade uživatele.</span><span class="sxs-lookup"><span data-stu-id="13486-192">hello objective of this section is toocreate a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="13486-193">AppBlade podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="13486-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="13486-194">**Ujistěte se, že je pro zřizování uživatelů AppBlade nakonfigurován název vaší domény. Po této pouze hello za běhu zřizování uživatelů funguje.**</span><span class="sxs-lookup"><span data-stu-id="13486-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only hello just-in-time user provisioning works.**</span></span>

<span data-ttu-id="13486-195">Pokud má uživatel hello e-mailovou adresu konče hello domény nakonfiguroval AppBlade pro váš účet, pak uživatel hello se automaticky připojí hello účet jako člena s hello úrovně, kterou zadáte, který je jedním z "Basic" (základní uživatel, který můžete nainstalovat pouze aplikace), "Člen týmu" (uživatel, který můžete nahrát nové verze aplikace a řízení projektů) nebo "Správce" (toohello účet oprávnění Úplné správce).</span><span class="sxs-lookup"><span data-stu-id="13486-195">If hello user has an email address ending with hello domain configured by AppBlade for your account, then hello user will automatically join hello account as a member with hello permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges toohello account).</span></span> <span data-ttu-id="13486-196">Za normálních okolností by zvolte Basic a pak zvýšení úrovně ručně přes k přihlášení správce (AppBlade předem musí tooconfigure buď k přihlášení na základě e-mailu správce nebo povýšit uživatele po přihlášení jménem zákazníka hello).</span><span class="sxs-lookup"><span data-stu-id="13486-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs tooconfigure either an email-based admin login in advance or promote a user on behalf of hello customer after login).</span></span>

<span data-ttu-id="13486-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="13486-197">There is no action item for you in this section.</span></span> <span data-ttu-id="13486-198">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess AppBlade.</span><span class="sxs-lookup"><span data-stu-id="13486-198">A new user is created during an attempt tooaccess AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="13486-199">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory AppBlade](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="13486-199">If you need toocreate a user manually, you need toocontact hello [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="13486-200">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="13486-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="13486-201">V této části povolíte tak, že udělíte přístup tooAppBlade toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="13486-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppBlade.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="13486-203">**tooassign Britta Simon tooAppBlade, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="13486-203">**tooassign Britta Simon tooAppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="13486-204">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="13486-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="13486-206">V seznamu aplikace hello vyberte **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="13486-206">In hello applications list, select **AppBlade**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="13486-208">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="13486-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="13486-210">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="13486-210">Click **Add** button.</span></span> <span data-ttu-id="13486-211">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13486-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="13486-213">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="13486-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="13486-214">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13486-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="13486-215">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="13486-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="13486-216">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="13486-216">Testing single sign-on</span></span>

<span data-ttu-id="13486-217">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="13486-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="13486-218">Když kliknete na dlaždici AppBlade hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour AppBlade aplikace.</span><span class="sxs-lookup"><span data-stu-id="13486-218">When you click hello AppBlade tile in hello Access Panel, you should get automatically signed-on tooyour AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="13486-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="13486-219">Additional resources</span></span>

* [<span data-ttu-id="13486-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="13486-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="13486-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="13486-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

