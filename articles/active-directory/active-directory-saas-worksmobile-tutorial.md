---
title: 'Kurz: Azure Active Directory integrace s MOBILE FUNGUJE | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a FUNGUJE MOBILE."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 80192218a2e99a921834bb53e708d5e4fab413f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a><span data-ttu-id="f0bdc-103">Kurz: Azure Active Directory integrace s MOBILE FUNGUJE</span><span class="sxs-lookup"><span data-stu-id="f0bdc-103">Tutorial: Azure Active Directory integration with WORKS MOBILE</span></span>

<span data-ttu-id="f0bdc-104">V tomto kurzu zjistíte, jak FUNGUJE toointegrate mobilní s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f0bdc-104">In this tutorial, you learn how toointegrate WORKS MOBILE with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f0bdc-105">Integrace MOBILE FUNGUJE s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-105">Integrating WORKS MOBILE with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f0bdc-106">Můžete řídit ve službě Azure AD, který má přístup tooWORKS MOBILE</span><span class="sxs-lookup"><span data-stu-id="f0bdc-106">You can control in Azure AD who has access tooWORKS MOBILE</span></span>
- <span data-ttu-id="f0bdc-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWORKS MOBILE (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bdc-107">You can enable your users tooautomatically get signed-on tooWORKS MOBILE (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f0bdc-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f0bdc-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f0bdc-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f0bdc-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0bdc-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0bdc-110">Prerequisites</span></span>

<span data-ttu-id="f0bdc-111">tooconfigure integrace Azure AD s MOBILE FUNGUJE, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-111">tooconfigure Azure AD integration with WORKS MOBILE, you need hello following items:</span></span>

- <span data-ttu-id="f0bdc-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bdc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f0bdc-113">MOBILE FUNGUJE jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f0bdc-113">A WORKS MOBILE single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f0bdc-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f0bdc-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f0bdc-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f0bdc-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0bdc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f0bdc-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f0bdc-118">Scenario description</span></span>
<span data-ttu-id="f0bdc-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f0bdc-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f0bdc-121">Přidání MOBILE FUNGUJE z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f0bdc-121">Adding WORKS MOBILE from hello gallery</span></span>
2. <span data-ttu-id="f0bdc-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0bdc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-works-mobile-from-hello-gallery"></a><span data-ttu-id="f0bdc-123">Přidání MOBILE FUNGUJE z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f0bdc-123">Adding WORKS MOBILE from hello gallery</span></span>
<span data-ttu-id="f0bdc-124">tooconfigure hello integrace MOBILE FUNGUJE do Azure AD, je nutné tooadd MOBILE FUNGUJE hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-124">tooconfigure hello integration of WORKS MOBILE into Azure AD, you need tooadd WORKS MOBILE from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f0bdc-125">**tooadd FUNGUJE MOBILE z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-125">**tooadd WORKS MOBILE from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0bdc-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f0bdc-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f0bdc-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f0bdc-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f0bdc-133">Hello vyhledávacího pole zadejte **MOBILE FUNGUJE**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-133">In hello search box, type **WORKS MOBILE**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. <span data-ttu-id="f0bdc-135">Na panelu výsledků hello vyberte **MOBILE FUNGUJE**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-135">In hello results panel, select **WORKS MOBILE**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f0bdc-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0bdc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f0bdc-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s MOBILE FUNGUJE podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f0bdc-138">In this section, you configure and test Azure AD single sign-on with WORKS MOBILE based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f0bdc-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v MOBILE FUNGUJE je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in WORKS MOBILE is tooa user in Azure AD.</span></span> <span data-ttu-id="f0bdc-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MOBILE FUNGUJE musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-140">In other words, a link relationship between an Azure AD user and hello related user in WORKS MOBILE needs toobe established.</span></span>

<span data-ttu-id="f0bdc-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in WORKS MOBILE.</span></span>

<span data-ttu-id="f0bdc-142">tooconfigure a testu Azure AD jednotné přihlašování s MOBILE FUNGUJE, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-142">tooconfigure and test Azure AD single sign-on with WORKS MOBILE, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f0bdc-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f0bdc-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f0bdc-145">**[Vytvoření zkušebního uživatele MOBILE FUNGUJE](#creating-a-works-mobile-test-user)**  -toohave protějšek Britta Simon v FUNGUJE MOBILE, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-145">**[Creating a WORKS MOBILE test user](#creating-a-works-mobile-test-user)** - toohave a counterpart of Britta Simon in WORKS MOBILE that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f0bdc-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f0bdc-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f0bdc-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0bdc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f0bdc-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci MOBILE FUNGUJE.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your WORKS MOBILE application.</span></span>

<span data-ttu-id="f0bdc-150">**tooconfigure Azure AD jednotné přihlašování s MOBILE FUNGUJE, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-150">**tooconfigure Azure AD single sign-on with WORKS MOBILE, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0bdc-151">V portálu Azure, na hello hello **MOBILE FUNGUJE** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-151">In hello Azure portal, on hello **WORKS MOBILE** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f0bdc-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. <span data-ttu-id="f0bdc-155">Na hello **FUNGUJE MOBILE domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-155">On hello **WORKS MOBILE Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    <span data-ttu-id="f0bdc-157">a.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-157">a.</span></span> <span data-ttu-id="f0bdc-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.worksmobile.com/jp/myservice`</span><span class="sxs-lookup"><span data-stu-id="f0bdc-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.worksmobile.com/jp/myservice`</span></span>

    <span data-ttu-id="f0bdc-159">b.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-159">b.</span></span> <span data-ttu-id="f0bdc-160">V hello **identifikátor** textovému poli, hodnota typu hello jako`worksmobile.com`</span><span class="sxs-lookup"><span data-stu-id="f0bdc-160">In hello **Identifier** textbox, type hello value as `worksmobile.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f0bdc-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-161">This value is not real.</span></span> <span data-ttu-id="f0bdc-162">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="f0bdc-163">Obraťte se na [tým podpory FUNGUJE MOBILNÍHO klienta](mailto:dl_ssoinfo@worksmobile.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-163">Contact [WORKS MOBILE Client support team](mailto:dl_ssoinfo@worksmobile.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="f0bdc-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-164">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. <span data-ttu-id="f0bdc-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f0bdc-168">Na hello **Konfigurace mobilních FUNGUJE** klikněte na tlačítko **nakonfigurovat mobilní FUNGUJE** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-168">On hello **WORKS MOBILE Configuration** section, click **Configure WORKS MOBILE** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f0bdc-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. <span data-ttu-id="f0bdc-171">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory FUNGUJE MOBILE](mailto:dl_ssoinfo@worksmobile.com) , poskytovat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-171">tooget SSO configured for your application, contact [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) and provide them with hello following information:</span></span> 

    <span data-ttu-id="f0bdc-172">• hello Stáhnout **soubor certifikátu**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-172">• hello downloaded **Certificate file**</span></span>

    <span data-ttu-id="f0bdc-173">• hello **SAML jeden přihlašování adresa URL služby**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-173">• hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="f0bdc-174">• hello **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-174">• hello **SAML Entity ID**</span></span>

    <span data-ttu-id="f0bdc-175">• hello **Sign-Out adresy URL**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-175">• hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="f0bdc-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f0bdc-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f0bdc-177">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f0bdc-178">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f0bdc-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f0bdc-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bdc-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="f0bdc-180">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f0bdc-182">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0bdc-183">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f0bdc-185">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f0bdc-187">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f0bdc-189">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0bdc-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f0bdc-191">a.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-191">a.</span></span> <span data-ttu-id="f0bdc-192">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f0bdc-193">b.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-193">b.</span></span> <span data-ttu-id="f0bdc-194">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f0bdc-195">c.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-195">c.</span></span> <span data-ttu-id="f0bdc-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f0bdc-197">d.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-197">d.</span></span> <span data-ttu-id="f0bdc-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-198">Click **Create**.</span></span>
 
### <a name="creating-a-works-mobile-test-user"></a><span data-ttu-id="f0bdc-199">Vytvoření zkušebního uživatele MOBILE FUNGUJE</span><span class="sxs-lookup"><span data-stu-id="f0bdc-199">Creating a WORKS MOBILE test user</span></span>

 <span data-ttu-id="f0bdc-200">V této části vytvoříte uživatele v MOBILE FUNGUJE jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-200">In this section, you create a user called Britta Simon in WORKS MOBILE.</span></span> <span data-ttu-id="f0bdc-201">Spojte se s [tým podpory FUNGUJE MOBILE](mailto:dl_ssoinfo@worksmobile.com) tooadd hello uživatelé hello FUNGUJE mobilní platformy.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-201">Please work with [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) tooadd hello users in hello WORKS MOBILE platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f0bdc-202">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f0bdc-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f0bdc-203">V této části povolíte tak, že udělíte přístup tooWORKS MOBILE Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWORKS MOBILE.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f0bdc-205">**tooassign Britta Simon tooWORKS mobilních, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f0bdc-205">**tooassign Britta Simon tooWORKS MOBILE, perform hello following steps:**</span></span>

1. <span data-ttu-id="f0bdc-206">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-206">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f0bdc-208">V seznamu aplikace hello vyberte **MOBILE FUNGUJE**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-208">In hello applications list, select **WORKS MOBILE**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. <span data-ttu-id="f0bdc-210">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f0bdc-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-212">Click **Add** button.</span></span> <span data-ttu-id="f0bdc-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f0bdc-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f0bdc-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f0bdc-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f0bdc-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0bdc-218">Testing single sign-on</span></span>

<span data-ttu-id="f0bdc-219">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-219">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="f0bdc-220">Když kliknete na dlaždici hello MOBILE FUNGUJE v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour FUNGUJE mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bdc-220">When you click hello WORKS MOBILE tile in hello Access Panel, you should get automatically signed-on tooyour WORKS MOBILE application.</span></span>
<span data-ttu-id="f0bdc-221">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0bdc-221">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f0bdc-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f0bdc-222">Additional resources</span></span>

* [<span data-ttu-id="f0bdc-223">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0bdc-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f0bdc-224">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f0bdc-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

