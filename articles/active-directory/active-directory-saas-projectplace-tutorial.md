---
title: 'Kurz: Azure Active Directory integrace s Projectplace | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Projectplace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 95d109052096161f995ff26a18f8d64f0c4a3dc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="381e5-103">Kurz: Azure Active Directory integrace s Projectplace</span><span class="sxs-lookup"><span data-stu-id="381e5-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="381e5-104">V tomto kurzu zjistíte, jak toointegrate Projectplace s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="381e5-104">In this tutorial, you learn how toointegrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="381e5-105">Integrace Projectplace s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="381e5-105">Integrating Projectplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="381e5-106">Můžete řídit ve službě Azure AD, který má přístup tooProjectplace</span><span class="sxs-lookup"><span data-stu-id="381e5-106">You can control in Azure AD who has access tooProjectplace</span></span>
- <span data-ttu-id="381e5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooProjectplace (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="381e5-107">You can enable your users tooautomatically get signed-on tooProjectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="381e5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="381e5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="381e5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="381e5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="381e5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="381e5-110">Prerequisites</span></span>

<span data-ttu-id="381e5-111">Integrace služby Azure AD s Projectplace tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="381e5-111">tooconfigure Azure AD integration with Projectplace, you need hello following items:</span></span>

- <span data-ttu-id="381e5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="381e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="381e5-113">Projectplace jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="381e5-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="381e5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="381e5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="381e5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="381e5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="381e5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="381e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="381e5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="381e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="381e5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="381e5-118">Scenario description</span></span>
<span data-ttu-id="381e5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="381e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="381e5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="381e5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="381e5-121">Přidání Projectplace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="381e5-121">Adding Projectplace from hello gallery</span></span>
2. <span data-ttu-id="381e5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="381e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-hello-gallery"></a><span data-ttu-id="381e5-123">Přidání Projectplace z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="381e5-123">Adding Projectplace from hello gallery</span></span>
<span data-ttu-id="381e5-124">tooconfigure hello integrace Projectplace do Azure AD, je nutné tooadd Projectplace hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="381e5-124">tooconfigure hello integration of Projectplace into Azure AD, you need tooadd Projectplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="381e5-125">**tooadd Projectplace z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="381e5-125">**tooadd Projectplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="381e5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="381e5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="381e5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="381e5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="381e5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="381e5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="381e5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="381e5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="381e5-133">Hello vyhledávacího pole zadejte **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="381e5-133">In hello search box, type **Projectplace**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="381e5-135">Na panelu výsledků hello vyberte **Projectplace**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="381e5-135">In hello results panel, select **Projectplace**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="381e5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="381e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="381e5-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Projectplace podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="381e5-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="381e5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Projectplace je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="381e5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Projectplace is tooa user in Azure AD.</span></span> <span data-ttu-id="381e5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Projectplace musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="381e5-140">In other words, a link relationship between an Azure AD user and hello related user in Projectplace needs toobe established.</span></span>

<span data-ttu-id="381e5-141">V Projectplace, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="381e5-141">In Projectplace, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="381e5-142">tooconfigure a testu Azure AD jednotné přihlašování s Projectplace, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="381e5-142">tooconfigure and test Azure AD single sign-on with Projectplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="381e5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="381e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="381e5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="381e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="381e5-145">**[Vytvoření zkušebního uživatele Projectplace](#creating-a-projectplace-test-user)**  -toohave protějšek Britta Simon v Projectplace, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="381e5-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - toohave a counterpart of Britta Simon in Projectplace that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="381e5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="381e5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="381e5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="381e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="381e5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="381e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="381e5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaše Projectplace aplikace.</span><span class="sxs-lookup"><span data-stu-id="381e5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="381e5-150">**tooconfigure Azure AD jednotné přihlašování s Projectplace, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="381e5-150">**tooconfigure Azure AD single sign-on with Projectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="381e5-151">V portálu Azure, na hello hello **Projectplace** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="381e5-151">In hello Azure portal, on hello **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="381e5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="381e5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="381e5-155">Na hello **Projectplace domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="381e5-155">On hello **Projectplace Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="381e5-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="381e5-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="381e5-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="381e5-158">This value is not real.</span></span> <span data-ttu-id="381e5-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="381e5-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="381e5-160">Obraťte se na [tým podpory Projectplace klienta](https://success.planview.com/Projectplace/Support) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="381e5-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) tooget this value.</span></span> 
 
4. <span data-ttu-id="381e5-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="381e5-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="381e5-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="381e5-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="381e5-165">tooconfigure jednotného přihlašování na **Projectplace** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Projectplace](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="381e5-165">tooconfigure single sign-on on **Projectplace** side, you need toosend hello downloaded **Metadata XML** too[Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="381e5-166">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="381e5-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="381e5-167">Hello jeden přihlašování konfigurace obsahuje toobe provádí hello [tým podpory Projectplace](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="381e5-167">hello single sign-on configuration has toobe performed by hello [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="381e5-168">Zobrazí se oznámení a také konfigurace hello se dokončila.</span><span class="sxs-lookup"><span data-stu-id="381e5-168">You will get a notification as soon as hello configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="381e5-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="381e5-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="381e5-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="381e5-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="381e5-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="381e5-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="381e5-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="381e5-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="381e5-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="381e5-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="381e5-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="381e5-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="381e5-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="381e5-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="381e5-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="381e5-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="381e5-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="381e5-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="381e5-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="381e5-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="381e5-184">a.</span><span class="sxs-lookup"><span data-stu-id="381e5-184">a.</span></span> <span data-ttu-id="381e5-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="381e5-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="381e5-186">b.</span><span class="sxs-lookup"><span data-stu-id="381e5-186">b.</span></span> <span data-ttu-id="381e5-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="381e5-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="381e5-188">c.</span><span class="sxs-lookup"><span data-stu-id="381e5-188">c.</span></span> <span data-ttu-id="381e5-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="381e5-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="381e5-190">d.</span><span class="sxs-lookup"><span data-stu-id="381e5-190">d.</span></span> <span data-ttu-id="381e5-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="381e5-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="381e5-192">Vytvoření zkušebního uživatele Projectplace</span><span class="sxs-lookup"><span data-stu-id="381e5-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="381e5-193">V pořadí tooenable Azure AD Uživatelé toolog do Projectplace musí být zřízená do Projectplace.</span><span class="sxs-lookup"><span data-stu-id="381e5-193">In order tooenable Azure AD users toolog into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="381e5-194">V případě hello Projectplace zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="381e5-194">In hello case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="381e5-195">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="381e5-195">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="381e5-196">Přihlaste se tooyour **Projectplace** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="381e5-196">Log in tooyour **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="381e5-197">Přejděte příliš**osoby**a potom klikněte na **členy**.</span><span class="sxs-lookup"><span data-stu-id="381e5-197">Go too**People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="381e5-198">![Lidé](./media/active-directory-saas-projectplace-tutorial/ic790228.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="381e5-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="381e5-199">Klikněte na tlačítko **přidat člena**.</span><span class="sxs-lookup"><span data-stu-id="381e5-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="381e5-200">![Přidání členů](./media/active-directory-saas-projectplace-tutorial/ic790232.png "přidat členy")</span><span class="sxs-lookup"><span data-stu-id="381e5-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="381e5-201">V hello **přidat člena** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="381e5-201">In hello **Add Member** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="381e5-202">![Nové členy](./media/active-directory-saas-projectplace-tutorial/ic790233.png "nové členy")</span><span class="sxs-lookup"><span data-stu-id="381e5-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="381e5-203">a.</span><span class="sxs-lookup"><span data-stu-id="381e5-203">a.</span></span> <span data-ttu-id="381e5-204">V hello **nové členy** textovému poli, typ hello e-mailovou adresu platného účtu AAD chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="381e5-204">In hello **New Members** textbox, type hello email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="381e5-205">b.</span><span class="sxs-lookup"><span data-stu-id="381e5-205">b.</span></span> <span data-ttu-id="381e5-206">Klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="381e5-206">Click **Send**.</span></span>

   <span data-ttu-id="381e5-207">Předtím, než se stane aktivní, včetně účet odkaz tooconfirm hello e-mail je odeslán toohello držitel účtu Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="381e5-207">An email including a link tooconfirm hello account before it becomes active is sent toohello Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="381e5-208">Můžete použít všechny ostatní Projectplace uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Projectplace tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="381e5-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="381e5-209">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="381e5-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="381e5-210">V této části povolíte tak, že udělíte přístup tooProjectplace toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="381e5-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooProjectplace.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="381e5-212">**tooassign Britta Simon tooProjectplace, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="381e5-212">**tooassign Britta Simon tooProjectplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="381e5-213">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="381e5-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="381e5-215">V seznamu aplikace hello vyberte **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="381e5-215">In hello applications list, select **Projectplace**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="381e5-217">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="381e5-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="381e5-219">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="381e5-219">Click **Add** button.</span></span> <span data-ttu-id="381e5-220">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="381e5-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="381e5-222">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="381e5-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="381e5-223">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="381e5-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="381e5-224">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="381e5-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="381e5-225">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="381e5-225">Testing single sign-on</span></span>

<span data-ttu-id="381e5-226">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="381e5-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="381e5-227">Když kliknete na dlaždici Projectplace hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Projectplace aplikace.</span><span class="sxs-lookup"><span data-stu-id="381e5-227">When you click hello Projectplace tile in hello Access Panel, you should get automatically signed-on tooyour Projectplace application.</span></span>
<span data-ttu-id="381e5-228">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="381e5-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="381e5-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="381e5-229">Additional resources</span></span>

* [<span data-ttu-id="381e5-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="381e5-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="381e5-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="381e5-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

