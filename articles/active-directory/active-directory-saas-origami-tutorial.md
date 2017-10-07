---
title: 'Kurz: Azure Active Directory integrace s Origami | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Origami."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a45f2d2b8d2271cf0fc58cb8fad92f007cb5e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a><span data-ttu-id="cd959-103">Kurz: Azure Active Directory integrace s Origami</span><span class="sxs-lookup"><span data-stu-id="cd959-103">Tutorial: Azure Active Directory integration with Origami</span></span>

<span data-ttu-id="cd959-104">V tomto kurzu zjistíte, jak toointegrate Origami s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cd959-104">In this tutorial, you learn how toointegrate Origami with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd959-105">Integrace Origami s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cd959-105">Integrating Origami with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cd959-106">Můžete řídit ve službě Azure AD, který má přístup tooOrigami</span><span class="sxs-lookup"><span data-stu-id="cd959-106">You can control in Azure AD who has access tooOrigami</span></span>
- <span data-ttu-id="cd959-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOrigami (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd959-107">You can enable your users tooautomatically get signed-on tooOrigami (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd959-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cd959-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cd959-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd959-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd959-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd959-110">Prerequisites</span></span>

<span data-ttu-id="cd959-111">Integrace služby Azure AD s Origami tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cd959-111">tooconfigure Azure AD integration with Origami, you need hello following items:</span></span>

- <span data-ttu-id="cd959-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd959-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd959-113">Origami jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cd959-113">An Origami single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd959-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd959-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd959-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cd959-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd959-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cd959-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd959-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd959-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd959-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cd959-118">Scenario description</span></span>
<span data-ttu-id="cd959-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cd959-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd959-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cd959-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd959-121">Přidání Origami z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd959-121">Adding Origami from hello gallery</span></span>
2. <span data-ttu-id="cd959-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd959-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-origami-from-hello-gallery"></a><span data-ttu-id="cd959-123">Přidání Origami z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cd959-123">Adding Origami from hello gallery</span></span>
<span data-ttu-id="cd959-124">tooconfigure hello integrace Origami do Azure AD, je nutné tooadd Origami hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cd959-124">tooconfigure hello integration of Origami into Azure AD, you need tooadd Origami from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cd959-125">**tooadd Origami z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd959-125">**tooadd Origami from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd959-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd959-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd959-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cd959-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cd959-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd959-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cd959-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd959-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cd959-133">Hello vyhledávacího pole zadejte **Origami**.</span><span class="sxs-lookup"><span data-stu-id="cd959-133">In hello search box, type **Origami**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. <span data-ttu-id="cd959-135">Na panelu výsledků hello vyberte **Origami**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd959-135">In hello results panel, select **Origami**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd959-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd959-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd959-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Origami podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cd959-138">In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cd959-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Origami je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd959-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Origami is tooa user in Azure AD.</span></span> <span data-ttu-id="cd959-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Origami musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cd959-140">In other words, a link relationship between an Azure AD user and hello related user in Origami needs toobe established.</span></span>

<span data-ttu-id="cd959-141">V Origami, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="cd959-141">In Origami, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cd959-142">tooconfigure a testu Azure AD jednotné přihlašování s Origami, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cd959-142">tooconfigure and test Azure AD single sign-on with Origami, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cd959-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cd959-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cd959-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd959-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd959-145">**[Vytváření testovacího uživatele Origami](#creating-an-origami-test-user)**  -toohave protějšek Britta Simon v Origami, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd959-145">**[Creating an Origami test user](#creating-an-origami-test-user)** - toohave a counterpart of Britta Simon in Origami that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd959-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd959-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd959-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cd959-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd959-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd959-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd959-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Origami.</span><span class="sxs-lookup"><span data-stu-id="cd959-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Origami application.</span></span>

<span data-ttu-id="cd959-150">**tooconfigure Azure AD jednotné přihlašování s Origami, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd959-150">**tooconfigure Azure AD single sign-on with Origami, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd959-151">V portálu Azure, na hello hello **Origami** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cd959-151">In hello Azure portal, on hello **Origami** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cd959-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd959-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. <span data-ttu-id="cd959-155">Na hello **Origami domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cd959-155">On hello **Origami Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    <span data-ttu-id="cd959-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://live.origamirisk.com/origami/account/login?account=<companyname>`</span><span class="sxs-lookup"><span data-stu-id="cd959-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd959-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="cd959-158">hello value is not real.</span></span> <span data-ttu-id="cd959-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd959-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="cd959-160">Obraťte se na [tým podpory Origami klienta](https://wordpress.org/support/theme/origami) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cd959-160">Contact [Origami Client support team](https://wordpress.org/support/theme/origami) tooget hello value.</span></span> 
 
4. <span data-ttu-id="cd959-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cd959-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. <span data-ttu-id="cd959-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd959-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cd959-165">Na hello **Origami konfigurace** klikněte na tlačítko **konfigurace Origami** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cd959-165">On hello **Origami Configuration** section, click **Configure Origami** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cd959-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cd959-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. <span data-ttu-id="cd959-168">Přihlaste se toohello Origami účet s právy správce.</span><span class="sxs-lookup"><span data-stu-id="cd959-168">Log in toohello Origami account with Admin rights.</span></span>

8. <span data-ttu-id="cd959-169">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="cd959-169">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. <span data-ttu-id="cd959-171">Na stránce dialogové okno jeden znak na instalace hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cd959-171">On hello Single Sign On Setup dialog page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    <span data-ttu-id="cd959-173">a.</span><span class="sxs-lookup"><span data-stu-id="cd959-173">a.</span></span> <span data-ttu-id="cd959-174">Vyberte **povolení jednotného přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="cd959-174">Select **Enable Single Sign On**.</span></span>

    <span data-ttu-id="cd959-175">b.</span><span class="sxs-lookup"><span data-stu-id="cd959-175">b.</span></span> <span data-ttu-id="cd959-176">V hello **zprostředkovatele Identity adresu URL přihlašovací stránky** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd959-176">In hello **Identity Provider's Sign-in Page URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cd959-177">c.</span><span class="sxs-lookup"><span data-stu-id="cd959-177">c.</span></span> <span data-ttu-id="cd959-178">V hello **adresa URL stránky Sign-out zprostředkovatele Identity** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd959-178">In hello **Identity Provider's Sign-out Page URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="cd959-179">d.</span><span class="sxs-lookup"><span data-stu-id="cd959-179">d.</span></span> <span data-ttu-id="cd959-180">Klikněte na tlačítko **Procházet** certifikát hello tooupload jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="cd959-180">Click **Browse** tooupload hello certificate you have downloaded from hello Azure portal.</span></span>

    <span data-ttu-id="cd959-181">e.</span><span class="sxs-lookup"><span data-stu-id="cd959-181">e.</span></span> <span data-ttu-id="cd959-182">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="cd959-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="cd959-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cd959-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cd959-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cd959-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cd959-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd959-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd959-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd959-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd959-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cd959-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cd959-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd959-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd959-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cd959-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd959-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cd959-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd959-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cd959-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd959-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cd959-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd959-198">a.</span><span class="sxs-lookup"><span data-stu-id="cd959-198">a.</span></span> <span data-ttu-id="cd959-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd959-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd959-200">b.</span><span class="sxs-lookup"><span data-stu-id="cd959-200">b.</span></span> <span data-ttu-id="cd959-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd959-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd959-202">c.</span><span class="sxs-lookup"><span data-stu-id="cd959-202">c.</span></span> <span data-ttu-id="cd959-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cd959-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cd959-204">d.</span><span class="sxs-lookup"><span data-stu-id="cd959-204">d.</span></span> <span data-ttu-id="cd959-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cd959-205">Click **Create**.</span></span>
 
### <a name="creating-an-origami-test-user"></a><span data-ttu-id="cd959-206">Vytváření testovacího uživatele Origami</span><span class="sxs-lookup"><span data-stu-id="cd959-206">Creating an Origami test user</span></span>

<span data-ttu-id="cd959-207">V této části vytvoříte volal Britta Simon v Origami uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd959-207">In this section, you create a user called Britta Simon in Origami.</span></span> 

1. <span data-ttu-id="cd959-208">Přihlaste se toohello Origami účet s právy správce.</span><span class="sxs-lookup"><span data-stu-id="cd959-208">Log in toohello Origami account with Admin rights.</span></span>

2. <span data-ttu-id="cd959-209">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="cd959-209">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. <span data-ttu-id="cd959-211">Na hello **uživatelů a zabezpečení** dialogové okno, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cd959-211">On hello **Users and Security** dialog, click **Users**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. <span data-ttu-id="cd959-213">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cd959-213">Click **Add New User**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. <span data-ttu-id="cd959-215">V dialogovém okně Přidat nové uživatele hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="cd959-215">On hello Add New User dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    <span data-ttu-id="cd959-217">a.</span><span class="sxs-lookup"><span data-stu-id="cd959-217">a.</span></span> <span data-ttu-id="cd959-218">V hello **uživatelské jméno** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cd959-218">In hello **User Name** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="cd959-219">b.</span><span class="sxs-lookup"><span data-stu-id="cd959-219">b.</span></span> <span data-ttu-id="cd959-220">V hello **heslo** textovému poli, zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="cd959-220">In hello **Password** textbox, type a password.</span></span>

    <span data-ttu-id="cd959-221">c.</span><span class="sxs-lookup"><span data-stu-id="cd959-221">c.</span></span> <span data-ttu-id="cd959-222">V hello **Potvrdit heslo** textovému poli, znovu zadejte heslo hello.</span><span class="sxs-lookup"><span data-stu-id="cd959-222">In hello **Confirm Password** textbox, type hello password again.</span></span>

    <span data-ttu-id="cd959-223">d.</span><span class="sxs-lookup"><span data-stu-id="cd959-223">d.</span></span> <span data-ttu-id="cd959-224">V hello **křestní jméno** textovému poli, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cd959-224">In hello **First Name** textbox, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="cd959-225">e.</span><span class="sxs-lookup"><span data-stu-id="cd959-225">e.</span></span> <span data-ttu-id="cd959-226">V hello **příjmení** textovému poli, zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cd959-226">In hello **Last Name** textbox, enter hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="cd959-227">f.</span><span class="sxs-lookup"><span data-stu-id="cd959-227">f.</span></span> <span data-ttu-id="cd959-228">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cd959-228">Click **Save**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. <span data-ttu-id="cd959-230">Přiřadit **role uživatele** a **klientský přístup** toohello uživatele.</span><span class="sxs-lookup"><span data-stu-id="cd959-230">Assign **User Roles** and **Client Access** toohello user.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cd959-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cd959-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cd959-233">V této části povolíte tak, že udělíte přístup tooOrigami toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cd959-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOrigami.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cd959-235">**tooassign Britta Simon tooOrigami, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cd959-235">**tooassign Britta Simon tooOrigami, perform hello following steps:**</span></span>

1. <span data-ttu-id="cd959-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cd959-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cd959-238">V seznamu aplikace hello vyberte **Origami**.</span><span class="sxs-lookup"><span data-stu-id="cd959-238">In hello applications list, select **Origami**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. <span data-ttu-id="cd959-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cd959-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cd959-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cd959-242">Click **Add** button.</span></span> <span data-ttu-id="cd959-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd959-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cd959-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cd959-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cd959-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd959-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd959-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cd959-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd959-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cd959-248">Testing single sign-on</span></span>

<span data-ttu-id="cd959-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cd959-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cd959-250">Když kliknete na dlaždici Origami hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Origami aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd959-250">When you click hello Origami tile in hello Access Panel, you should get automatically signed-on tooyour Origami application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd959-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cd959-251">Additional resources</span></span>

* [<span data-ttu-id="cd959-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd959-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd959-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd959-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

