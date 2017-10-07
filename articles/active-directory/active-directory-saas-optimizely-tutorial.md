---
title: 'Kurz: Azure Active Directory integrace s Optimizely | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Optimizely."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 868aefae27ca155d2963f3dcfcd79bbb564b48ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a><span data-ttu-id="70af0-103">Kurz: Azure Active Directory integrace s Optimizely</span><span class="sxs-lookup"><span data-stu-id="70af0-103">Tutorial: Azure Active Directory integration with Optimizely</span></span>

<span data-ttu-id="70af0-104">V tomto kurzu zjistíte, jak toointegrate Optimizely s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="70af0-104">In this tutorial, you learn how toointegrate Optimizely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70af0-105">Integrace Optimizely s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="70af0-105">Integrating Optimizely with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="70af0-106">Můžete řídit ve službě Azure AD, který má přístup tooOptimizely</span><span class="sxs-lookup"><span data-stu-id="70af0-106">You can control in Azure AD who has access tooOptimizely</span></span>
- <span data-ttu-id="70af0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOptimizely (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="70af0-107">You can enable your users tooautomatically get signed-on tooOptimizely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70af0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="70af0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="70af0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70af0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70af0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="70af0-110">Prerequisites</span></span>

<span data-ttu-id="70af0-111">Integrace služby Azure AD s Optimizely tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="70af0-111">tooconfigure Azure AD integration with Optimizely, you need hello following items:</span></span>

- <span data-ttu-id="70af0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="70af0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70af0-113">Optimizely jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="70af0-113">An Optimizely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70af0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="70af0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70af0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="70af0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70af0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="70af0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70af0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70af0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70af0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="70af0-118">Scenario description</span></span>
<span data-ttu-id="70af0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="70af0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70af0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="70af0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70af0-121">Přidání Optimizely z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70af0-121">Adding Optimizely from hello gallery</span></span>
2. <span data-ttu-id="70af0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70af0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-optimizely-from-hello-gallery"></a><span data-ttu-id="70af0-123">Přidání Optimizely z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="70af0-123">Adding Optimizely from hello gallery</span></span>
<span data-ttu-id="70af0-124">tooconfigure hello integrace Optimizely do Azure AD, je nutné tooadd Optimizely hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="70af0-124">tooconfigure hello integration of Optimizely into Azure AD, you need tooadd Optimizely from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="70af0-125">**tooadd Optimizely z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70af0-125">**tooadd Optimizely from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="70af0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70af0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70af0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="70af0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="70af0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70af0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="70af0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70af0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="70af0-133">Hello vyhledávacího pole zadejte **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="70af0-133">In hello search box, type **Optimizely**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. <span data-ttu-id="70af0-135">Na panelu výsledků hello vyberte **Optimizely**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="70af0-135">In hello results panel, select **Optimizely**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70af0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="70af0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70af0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Optimizely podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="70af0-138">In this section, you configure and test Azure AD single sign-on with Optimizely based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="70af0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Optimizely je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70af0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Optimizely is tooa user in Azure AD.</span></span> <span data-ttu-id="70af0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Optimizely musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="70af0-140">In other words, a link relationship between an Azure AD user and hello related user in Optimizely needs toobe established.</span></span>

<span data-ttu-id="70af0-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Optimizely.</span><span class="sxs-lookup"><span data-stu-id="70af0-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Optimizely.</span></span>

<span data-ttu-id="70af0-142">tooconfigure a testu Azure AD jednotné přihlašování s Optimizely, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="70af0-142">tooconfigure and test Azure AD single sign-on with Optimizely, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="70af0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="70af0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="70af0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70af0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70af0-145">**[Vytváření testovacího uživatele Optimizely](#creating-an-optimizely-test-user)**  -toohave protějšek Britta Simon v Optimizely, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="70af0-145">**[Creating an Optimizely test user](#creating-an-optimizely-test-user)** - toohave a counterpart of Britta Simon in Optimizely that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="70af0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70af0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70af0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="70af0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70af0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70af0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70af0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Optimizely.</span><span class="sxs-lookup"><span data-stu-id="70af0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Optimizely application.</span></span>

<span data-ttu-id="70af0-150">**tooconfigure Azure AD jednotné přihlašování s Optimizely, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70af0-150">**tooconfigure Azure AD single sign-on with Optimizely, perform hello following steps:**</span></span>

1. <span data-ttu-id="70af0-151">V portálu Azure, na hello hello **Optimizely** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="70af0-151">In hello Azure portal, on hello **Optimizely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="70af0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70af0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. <span data-ttu-id="70af0-155">Na hello **Optimizely domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="70af0-155">On hello **Optimizely Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    <span data-ttu-id="70af0-157">a.</span><span class="sxs-lookup"><span data-stu-id="70af0-157">a.</span></span> <span data-ttu-id="70af0-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://app.optimizely.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="70af0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.optimizely.net/<instance name>`</span></span>

    <span data-ttu-id="70af0-159">b.</span><span class="sxs-lookup"><span data-stu-id="70af0-159">b.</span></span> <span data-ttu-id="70af0-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`urn:auth0:optimizely:contoso`</span><span class="sxs-lookup"><span data-stu-id="70af0-160">In hello **Identifier** textbox, type a URL using hello following pattern:  `urn:auth0:optimizely:contoso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="70af0-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="70af0-161">These values are not hello real.</span></span> <span data-ttu-id="70af0-162">Aktualizujte hodnotu hello s hello skutečné přihlašovací adresa URL a identifikátor, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="70af0-162">You will update hello value with hello actual Sign-on URL and Identifier, which is explained later in hello tutorial.</span></span> 

4. <span data-ttu-id="70af0-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="70af0-163">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. <span data-ttu-id="70af0-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70af0-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70af0-167">Na hello **Optimizely konfigurace** klikněte na tlačítko **konfigurace Optimizely** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="70af0-167">On hello **Optimizely Configuration** section, click **Configure Optimizely** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="70af0-168">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="70af0-168">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. <span data-ttu-id="70af0-170">tooconfigure jednotného přihlašování na **Optimizely** straně, obraťte se na správce svého účtu Optimizely a zadejte hello Stáhnout **certifikátu (Base64)**, a **SAML jeden přihlašování adresa URL služby** .</span><span class="sxs-lookup"><span data-stu-id="70af0-170">tooconfigure single sign-on on **Optimizely** side, contact your Optimizely Account Manager and provide hello downloaded **Certificate (Base64)**, and **SAML Single Sign-On Service URL**.</span></span> 

8. <span data-ttu-id="70af0-171">V odpovědi tooyour e-mailu Optimizely vám poskytne hello přihlašovací adresa URL (jednotné přihlašování iniciované SP) a hello hodnot identifikátoru (ID Entity poskytovatele služby).</span><span class="sxs-lookup"><span data-stu-id="70af0-171">In response tooyour email, Optimizely provides you with hello Sign On URL (SP-initiated SSO) and hello Identifier (Service Provider Entity ID) values.</span></span>

    <span data-ttu-id="70af0-172">a.</span><span class="sxs-lookup"><span data-stu-id="70af0-172">a.</span></span> <span data-ttu-id="70af0-173">Kopírování hello **URL jednotné přihlašování iniciované SP** zadaný Optimizely a vložit do hello **přihlašovací adresa URL** textového pole v **Optimizely domény a adresy URL** části na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="70af0-173">Copy hello **SP-initiated SSO URL** provided by Optimizely, and paste into hello **Sign On URL** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

    <span data-ttu-id="70af0-174">b.</span><span class="sxs-lookup"><span data-stu-id="70af0-174">b.</span></span> <span data-ttu-id="70af0-175">Kopírování hello **ID Entity poskytovatele služby** zadaný Optimizely a vložit do hello **identifikátor** textového pole v **Optimizely domény a adresy URL** části na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="70af0-175">Copy hello **Service Provider Entity ID** provided by Optimizely, and paste into hello **Identifier** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

9. <span data-ttu-id="70af0-176">V okně jiný prohlížeč, přihlášení tooyour Optimizely aplikace.</span><span class="sxs-lookup"><span data-stu-id="70af0-176">In a different browser window, sign-on tooyour Optimizely application.</span></span>

10. <span data-ttu-id="70af0-177">Klikněte na účet pravém rohu název v horní části hello a potom **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="70af0-177">Click you account name in hello top right corner and then **Account Settings**.</span></span>
   
    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. <span data-ttu-id="70af0-179">Na kartě účet hello, zaškrtněte políčko hello **povolit jednotné přihlašování** pod jednotné přihlašování v hello **přehled** části.</span><span class="sxs-lookup"><span data-stu-id="70af0-179">In hello Account tab, check hello box **Enable SSO** under Single Sign On in hello **Overview** section.</span></span>
   
    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. <span data-ttu-id="70af0-181">Klikněte na tlačítko **uložit**</span><span class="sxs-lookup"><span data-stu-id="70af0-181">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="70af0-182">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="70af0-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="70af0-183">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="70af0-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="70af0-184">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70af0-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70af0-185">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="70af0-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="70af0-186">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="70af0-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="70af0-188">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70af0-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="70af0-189">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="70af0-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70af0-191">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="70af0-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70af0-193">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="70af0-193">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70af0-195">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="70af0-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70af0-197">a.</span><span class="sxs-lookup"><span data-stu-id="70af0-197">a.</span></span> <span data-ttu-id="70af0-198">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70af0-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70af0-199">b.</span><span class="sxs-lookup"><span data-stu-id="70af0-199">b.</span></span> <span data-ttu-id="70af0-200">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70af0-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="70af0-201">c.</span><span class="sxs-lookup"><span data-stu-id="70af0-201">c.</span></span> <span data-ttu-id="70af0-202">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="70af0-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="70af0-203">d.</span><span class="sxs-lookup"><span data-stu-id="70af0-203">d.</span></span> <span data-ttu-id="70af0-204">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="70af0-204">Click **Create**.</span></span>
 
### <a name="creating-an-optimizely-test-user"></a><span data-ttu-id="70af0-205">Vytváření testovacího uživatele Optimizely</span><span class="sxs-lookup"><span data-stu-id="70af0-205">Creating an Optimizely test user</span></span>

<span data-ttu-id="70af0-206">V této části vytvoříte volal Britta Simon v Optimizely uživatele.</span><span class="sxs-lookup"><span data-stu-id="70af0-206">In this section, you create a user called Britta Simon in Optimizely.</span></span>

1. <span data-ttu-id="70af0-207">Na domovské stránce hello vyberte **spolupracovníci** kartě.</span><span class="sxs-lookup"><span data-stu-id="70af0-207">On hello home page, select **Collaborators** tab.</span></span>

2. <span data-ttu-id="70af0-208">tooadd nový projekt toohello spolupracovník, klikněte na tlačítko **nové spolupracovník**.</span><span class="sxs-lookup"><span data-stu-id="70af0-208">tooadd new collaborator toohello project, click **New Collaborator**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. <span data-ttu-id="70af0-210">Vyplňte hello e-mailovou adresu a přiřadit jim roli.</span><span class="sxs-lookup"><span data-stu-id="70af0-210">Fill in hello email address and assign them a role.</span></span> <span data-ttu-id="70af0-211">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="70af0-211">Click **Invite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. <span data-ttu-id="70af0-213">Zobrazí se jim pozvání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="70af0-213">They receive an email invite.</span></span> <span data-ttu-id="70af0-214">Pomocí hello e-mailovou adresu, mají toolog v tooOptimizely.</span><span class="sxs-lookup"><span data-stu-id="70af0-214">Using hello email address, they have toolog in tooOptimizely.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="70af0-215">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="70af0-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="70af0-216">V této části povolíte tak, že udělíte přístup tooOptimizely toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="70af0-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOptimizely.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="70af0-218">**tooassign Britta Simon tooOptimizely, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="70af0-218">**tooassign Britta Simon tooOptimizely, perform hello following steps:**</span></span>

1. <span data-ttu-id="70af0-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="70af0-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="70af0-221">V seznamu aplikace hello vyberte **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="70af0-221">In hello applications list, select **Optimizely**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. <span data-ttu-id="70af0-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="70af0-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="70af0-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="70af0-225">Click **Add** button.</span></span> <span data-ttu-id="70af0-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70af0-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="70af0-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="70af0-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="70af0-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70af0-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70af0-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="70af0-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70af0-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="70af0-231">Testing single sign-on</span></span>

<span data-ttu-id="70af0-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="70af0-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="70af0-233">Když kliknete na dlaždici Optimizely hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Optimizely aplikace.</span><span class="sxs-lookup"><span data-stu-id="70af0-233">When you click hello Optimizely tile in hello Access Panel, you should get automatically signed-on tooyour Optimizely application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="70af0-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="70af0-234">Additional resources</span></span>

* [<span data-ttu-id="70af0-235">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="70af0-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70af0-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70af0-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

