---
title: "Kurz: Azure Active Directory integrace s Birst agilní obchodní analýza | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Birst agilní obchodní analýzy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: f007edcec0fb8ece215ab69f7ec7ca59ca34bddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a><span data-ttu-id="baaf5-103">Kurz: Azure Active Directory integrace s Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="baaf5-103">Tutorial: Azure Active Directory integration with Birst Agile Business Analytics</span></span>

<span data-ttu-id="baaf5-104">V tomto kurzu zjistíte, jak toointegrate Birst Agile obchodní analýza s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="baaf5-104">In this tutorial, you learn how toointegrate Birst Agile Business Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="baaf5-105">Obchodní analýza agilní Birst integrace s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="baaf5-105">Integrating Birst Agile Business Analytics with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="baaf5-106">Můžete řídit ve službě Azure AD, který má přístup tooBirst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="baaf5-106">You can control in Azure AD who has access tooBirst Agile Business Analytics</span></span>
- <span data-ttu-id="baaf5-107">Vaši uživatelé tooautomatically get přihlášeného tooBirst agilní obchodní analýza (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="baaf5-107">You can enable your users tooautomatically get signed-on tooBirst Agile Business Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="baaf5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="baaf5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="baaf5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="baaf5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="baaf5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="baaf5-110">Prerequisites</span></span>

<span data-ttu-id="baaf5-111">tooconfigure integrace Azure AD s Birst agilní obchodní analýza, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="baaf5-111">tooconfigure Azure AD integration with Birst Agile Business Analytics, you need hello following items:</span></span>

- <span data-ttu-id="baaf5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="baaf5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="baaf5-113">Birst agilní obchodní analýza jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="baaf5-113">A Birst Agile Business Analytics single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="baaf5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="baaf5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="baaf5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="baaf5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="baaf5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="baaf5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="baaf5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="baaf5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="baaf5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="baaf5-118">Scenario description</span></span>
<span data-ttu-id="baaf5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="baaf5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="baaf5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="baaf5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="baaf5-121">Přidání z Galerie hello Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="baaf5-121">Adding Birst Agile Business Analytics from hello gallery</span></span>
2. <span data-ttu-id="baaf5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="baaf5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-birst-agile-business-analytics-from-hello-gallery"></a><span data-ttu-id="baaf5-123">Přidání z Galerie hello Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="baaf5-123">Adding Birst Agile Business Analytics from hello gallery</span></span>
<span data-ttu-id="baaf5-124">tooconfigure hello integrace Birst agilní obchodních analýz do Azure AD, je nutné tooadd Birst Agile obchodní analýza hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="baaf5-124">tooconfigure hello integration of Birst Agile Business Analytics into Azure AD, you need tooadd Birst Agile Business Analytics from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="baaf5-125">**tooadd Birst Agile obchodní analýza z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="baaf5-125">**tooadd Birst Agile Business Analytics from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="baaf5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="baaf5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="baaf5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="baaf5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="baaf5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="baaf5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="baaf5-133">Hello vyhledávacího pole zadejte **Birst Agile obchodní analýza**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-133">In hello search box, type **Birst Agile Business Analytics**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_search.png)

5. <span data-ttu-id="baaf5-135">Na panelu výsledků hello vyberte **Birst Agile obchodní analýza**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="baaf5-135">In hello results panel, select **Birst Agile Business Analytics**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="baaf5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="baaf5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="baaf5-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Birst agilní obchodní analýza podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="baaf5-138">In this section, you configure and test Azure AD single sign-on with Birst Agile Business Analytics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="baaf5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Birst agilní obchodní analýza je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="baaf5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Birst Agile Business Analytics is tooa user in Azure AD.</span></span> <span data-ttu-id="baaf5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Birst agilní obchodní analýza musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="baaf5-140">In other words, a link relationship between an Azure AD user and hello related user in Birst Agile Business Analytics needs toobe established.</span></span>

<span data-ttu-id="baaf5-141">V obchodní analýza agilní Birst přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="baaf5-141">In Birst Agile Business Analytics, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="baaf5-142">tooconfigure a testu Azure AD jednotné přihlašování s Birst agilní obchodní analýza, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="baaf5-142">tooconfigure and test Azure AD single sign-on with Birst Agile Business Analytics, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="baaf5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="baaf5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="baaf5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="baaf5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="baaf5-145">**[Vytvoření zkušebního uživatele Birst agilní obchodní analýza](#creating-a-birst-agile-business-analytics-test-user)**  -toohave protějšek Britta Simon v Birst agilní obchodních analýz, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="baaf5-145">**[Creating a Birst Agile Business Analytics test user](#creating-a-birst-agile-business-analytics-test-user)** - toohave a counterpart of Britta Simon in Birst Agile Business Analytics that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="baaf5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="baaf5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="baaf5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="baaf5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="baaf5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="baaf5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="baaf5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Birst agilní obchodní analýzy.</span><span class="sxs-lookup"><span data-stu-id="baaf5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Birst Agile Business Analytics application.</span></span>

<span data-ttu-id="baaf5-150">**tooconfigure Azure AD jednotné přihlašování s Birst agilní obchodní analýza proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="baaf5-150">**tooconfigure Azure AD single sign-on with Birst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="baaf5-151">V portálu Azure, na hello hello **Birst Agile obchodní analýza** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-151">In hello Azure portal, on hello **Birst Agile Business Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="baaf5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="baaf5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_samlbase.png)

3. <span data-ttu-id="baaf5-155">Na hello **Birst Agile obchodní analýzy domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="baaf5-155">On hello **Birst Agile Business Analytics Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_url.png)

     <span data-ttu-id="baaf5-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="baaf5-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

     <span data-ttu-id="baaf5-158">Adresa URL Hello závisí na hello datacenter nachází účtu Birst:</span><span class="sxs-lookup"><span data-stu-id="baaf5-158">hello URL depends on hello datacenter that your Birst account is located:</span></span> 

     * <span data-ttu-id="baaf5-159">Pro USA datacenter postupujte podle následujícího hello vzoru:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="baaf5-159">For US datacenter use following hello pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span> 

     * <span data-ttu-id="baaf5-160">Pro Evropu datacenter použijte následující vzor hello:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="baaf5-160">For Europe datacenter use hello following pattern: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="baaf5-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="baaf5-161">This value is not real.</span></span> <span data-ttu-id="baaf5-162">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="baaf5-162">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="baaf5-163">Obraťte se na [tým podpory Birst agilní obchodní analýzy klienta](mailto:info@birst.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="baaf5-163">Contact [Birst Agile Business Analytics Client support team](mailto:info@birst.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="baaf5-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="baaf5-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_certificate.png) 

5. <span data-ttu-id="baaf5-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="baaf5-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="baaf5-168">Na hello **Birst agilní konfigurace obchodní analýzy** klikněte na tlačítko **konfigurace Birst agilní obchodní analýza** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="baaf5-168">On hello **Birst Agile Business Analytics Configuration** section, click **Configure Birst Agile Business Analytics** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="baaf5-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="baaf5-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_configure.png) 

7. <span data-ttu-id="baaf5-171">tooconfigure jednotného přihlašování na **Birst Agile obchodní analýza** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jednotné přihlašování Adresa URL služby** příliš[tým podpory Birst agilní obchodní analýza](mailto:info@birst.com).</span><span class="sxs-lookup"><span data-stu-id="baaf5-171">tooconfigure single sign-on on **Birst Agile Business Analytics** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Birst Agile Business Analytics support team](mailto:info@birst.com).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="baaf5-172">Zmínili tooBirst týmu, že tato integrace potřebuje algoritmus SHA256 (SHA1 nebude podporuje), aby nastavují hello jednotného přihlašování na příslušný server hello jako **app2101** atd.</span><span class="sxs-lookup"><span data-stu-id="baaf5-172">Mention tooBirst team that this integration needs SHA256 Algorithm (SHA1 will not be supported) so that they can set hello SSO on hello appropriate server like **app2101** etc.</span></span>
  

> [!TIP]
> <span data-ttu-id="baaf5-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="baaf5-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="baaf5-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="baaf5-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="baaf5-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="baaf5-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="baaf5-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="baaf5-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="baaf5-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="baaf5-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="baaf5-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="baaf5-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="baaf5-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="baaf5-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="baaf5-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="baaf5-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="baaf5-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="baaf5-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="baaf5-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="baaf5-188">a.</span><span class="sxs-lookup"><span data-stu-id="baaf5-188">a.</span></span> <span data-ttu-id="baaf5-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="baaf5-190">b.</span><span class="sxs-lookup"><span data-stu-id="baaf5-190">b.</span></span> <span data-ttu-id="baaf5-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="baaf5-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="baaf5-192">c.</span><span class="sxs-lookup"><span data-stu-id="baaf5-192">c.</span></span> <span data-ttu-id="baaf5-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="baaf5-194">d.</span><span class="sxs-lookup"><span data-stu-id="baaf5-194">d.</span></span> <span data-ttu-id="baaf5-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-195">Click **Create**.</span></span>
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a><span data-ttu-id="baaf5-196">Vytvoření zkušebního uživatele Birst agilní obchodní analýza</span><span class="sxs-lookup"><span data-stu-id="baaf5-196">Creating a Birst Agile Business Analytics test user</span></span>

<span data-ttu-id="baaf5-197">Hello cílem této části je toocreate uživatel volal Britta Simon v Birst agilní obchodní analýzy.</span><span class="sxs-lookup"><span data-stu-id="baaf5-197">hello objective of this section is toocreate a user called Britta Simon in Birst Agile Business Analytics.</span></span> <span data-ttu-id="baaf5-198">Práce s [tým podpory Birst agilní obchodní analýza](mailto:info@birst.com) tooadd hello uživatele v hello Birst účtu.</span><span class="sxs-lookup"><span data-stu-id="baaf5-198">Work with [Birst Agile Business Analytics support team](mailto:info@birst.com) tooadd hello users in hello Birst account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="baaf5-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="baaf5-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="baaf5-200">V této části povolíte tak, že udělíte přístup tooBirst agilní obchodní analýza Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="baaf5-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBirst Agile Business Analytics.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="baaf5-202">**tooassign tooBirst Britta Simon agilní obchodní analýza, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="baaf5-202">**tooassign Britta Simon tooBirst Agile Business Analytics, perform hello following steps:**</span></span>

1. <span data-ttu-id="baaf5-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="baaf5-205">V seznamu aplikace hello vyberte **Birst Agile obchodní analýza**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-205">In hello applications list, select **Birst Agile Business Analytics**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-birst-tutorial/tutorial_birst_app.png) 

3. <span data-ttu-id="baaf5-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="baaf5-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="baaf5-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="baaf5-209">Click **Add** button.</span></span> <span data-ttu-id="baaf5-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="baaf5-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="baaf5-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="baaf5-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="baaf5-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="baaf5-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="baaf5-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="baaf5-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="baaf5-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="baaf5-215">Testing single sign-on</span></span>

<span data-ttu-id="baaf5-216">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="baaf5-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="baaf5-217">Po kliknutí na tlačítko hello Birst agilní obchodní analýza dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Birst Agile obchodní analýza aplikace.</span><span class="sxs-lookup"><span data-stu-id="baaf5-217">When you click hello Birst Agile Business Analytics tile in hello Access Panel, you should get automatically signed-on tooyour Birst Agile Business Analytics application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="baaf5-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="baaf5-218">Additional resources</span></span>

* [<span data-ttu-id="baaf5-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="baaf5-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="baaf5-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="baaf5-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-birst-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png

