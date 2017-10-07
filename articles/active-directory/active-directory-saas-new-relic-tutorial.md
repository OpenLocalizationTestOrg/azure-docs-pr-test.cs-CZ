---
title: 'Kurz: Azure Active Directory integrace s New Relic. | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a New Relic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: dc8f0df3b18a32bde155e8911a581fc5f91af217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="7d5fb-103">Kurz: Azure Active Directory integrace s New Relic.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="7d5fb-104">V tomto kurzu zjistíte, jak toointegrate New Relic se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7d5fb-104">In this tutorial, you learn how toointegrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7d5fb-105">Integrace New Relic s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-105">Integrating New Relic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7d5fb-106">Můžete řídit ve službě Azure AD, který má přístup tooNew Relic</span><span class="sxs-lookup"><span data-stu-id="7d5fb-106">You can control in Azure AD who has access tooNew Relic</span></span>
- <span data-ttu-id="7d5fb-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNew Relic (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d5fb-107">You can enable your users tooautomatically get signed-on tooNew Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7d5fb-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7d5fb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7d5fb-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7d5fb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d5fb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7d5fb-110">Prerequisites</span></span>

<span data-ttu-id="7d5fb-111">tooconfigure integrace Azure AD s New Relic, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-111">tooconfigure Azure AD integration with New Relic, you need hello following items:</span></span>

- <span data-ttu-id="7d5fb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d5fb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7d5fb-113">New Relic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7d5fb-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7d5fb-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7d5fb-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7d5fb-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7d5fb-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d5fb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7d5fb-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7d5fb-118">Scenario description</span></span>
<span data-ttu-id="7d5fb-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7d5fb-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7d5fb-121">Přidání New Relic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7d5fb-121">Adding New Relic from hello gallery</span></span>
2. <span data-ttu-id="7d5fb-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d5fb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-hello-gallery"></a><span data-ttu-id="7d5fb-123">Přidání New Relic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7d5fb-123">Adding New Relic from hello gallery</span></span>
<span data-ttu-id="7d5fb-124">tooconfigure hello integrace New Relic do Azure AD, je nutné tooadd New Relic hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-124">tooconfigure hello integration of New Relic into Azure AD, you need tooadd New Relic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7d5fb-125">**tooadd New Relic z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-125">**tooadd New Relic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d5fb-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7d5fb-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7d5fb-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7d5fb-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7d5fb-133">Hello vyhledávacího pole zadejte **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-133">In hello search box, type **New Relic**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="7d5fb-135">Na panelu výsledků hello vyberte **New Relic**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-135">In hello results panel, select **New Relic**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7d5fb-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d5fb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7d5fb-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s New Relic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7d5fb-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7d5fb-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v New Relic je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in New Relic is tooa user in Azure AD.</span></span> <span data-ttu-id="7d5fb-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v New Relic musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-140">In other words, a link relationship between an Azure AD user and hello related user in New Relic needs toobe established.</span></span>

<span data-ttu-id="7d5fb-141">V New Relic, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-141">In New Relic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7d5fb-142">tooconfigure a testu Azure AD jednotné přihlašování s New Relic, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-142">tooconfigure and test Azure AD single sign-on with New Relic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7d5fb-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7d5fb-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7d5fb-145">**[Vytvoření zkušebního uživatele New Relic](#creating-a-new-relic-test-user)**  -toohave protějšek Britta Simon v New Relic, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - toohave a counterpart of Britta Simon in New Relic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7d5fb-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7d5fb-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7d5fb-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d5fb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7d5fb-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci New Relic.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="7d5fb-150">**tooconfigure Azure AD jednotné přihlašování s New Relic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-150">**tooconfigure Azure AD single sign-on with New Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d5fb-151">V portálu Azure, na hello hello **New Relic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-151">In hello Azure portal, on hello **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7d5fb-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="7d5fb-155">Na hello **nové domény Relic a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-155">On hello **New Relic Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="7d5fb-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="7d5fb-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7d5fb-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-158">hello value is not real.</span></span> <span data-ttu-id="7d5fb-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7d5fb-160">Obraťte se na [tým podpory pro nového klienta Relic](https://support.newrelic.com/) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-160">Contact [New Relic Client support team](https://support.newrelic.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="7d5fb-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="7d5fb-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7d5fb-165">Na hello **novou konfiguraci Relic** klikněte na tlačítko **konfigurace New Relic** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-165">On hello **New Relic Configuration** section, click **Configure New Relic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7d5fb-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="7d5fb-168">V okně prohlížeče jiných webových přihlásit tooyour **New Relic** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-168">In a different web browser window, sign on tooyour **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="7d5fb-169">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-169">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="7d5fb-170">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797036.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="7d5fb-171">Klikněte na tlačítko hello **zabezpečení a ověřování** a pak klikněte hello **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-171">Click hello **Security and authentication** tab, and then click hello **Single sign on** tab.</span></span>
   
    <span data-ttu-id="7d5fb-172">![Jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/ic797037.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="7d5fb-173">Na stránce dialogové okno hello SAML proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-173">On hello SAML dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="7d5fb-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="7d5fb-175">a.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-175">a.</span></span> <span data-ttu-id="7d5fb-176">Klikněte na tlačítko **zvolit soubor** tooupload vaše stažený certifikát Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-176">Click **Choose File** tooupload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="7d5fb-177">b.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-177">b.</span></span> <span data-ttu-id="7d5fb-178">V hello **adresu URL pro vzdálené přihlášení** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-178">In hello **Remote login URL** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="7d5fb-179">c.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-179">c.</span></span> <span data-ttu-id="7d5fb-180">V hello **cílová stránka adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-180">In hello **Logout landing URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="7d5fb-181">d.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-181">d.</span></span> <span data-ttu-id="7d5fb-182">Klikněte na tlačítko **uložit změny provedené v**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="7d5fb-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7d5fb-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7d5fb-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7d5fb-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7d5fb-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7d5fb-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d5fb-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="7d5fb-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7d5fb-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d5fb-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7d5fb-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7d5fb-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7d5fb-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7d5fb-198">a.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-198">a.</span></span> <span data-ttu-id="7d5fb-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7d5fb-200">b.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-200">b.</span></span> <span data-ttu-id="7d5fb-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7d5fb-202">c.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-202">c.</span></span> <span data-ttu-id="7d5fb-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7d5fb-204">d.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-204">d.</span></span> <span data-ttu-id="7d5fb-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="7d5fb-206">Vytvoření zkušebního uživatele New Relic.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-206">Creating a New Relic test user</span></span>

<span data-ttu-id="7d5fb-207">V pořadí tooenable Azure Active Directory uživatelům toolog v tooNew New Relic musí být zřízená do New Relic.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-207">In order tooenable Azure Active Directory users toolog in tooNew Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="7d5fb-208">V případě hello New Relic zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-208">In hello case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="7d5fb-209">**tooprovision tooNew uživatel účet New Relic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-209">**tooprovision a user account tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d5fb-210">Přihlaste se tooyour **New Relic** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-210">Log in tooyour **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="7d5fb-211">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení účtu**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-211">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="7d5fb-212">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797040.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="7d5fb-213">V hello **účet** podokně na hello levou stranu, klikněte na tlačítko **Souhrn**a potom klikněte na **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-213">In hello **Account** pane on hello left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="7d5fb-214">![Nastavení účtu](./media/active-directory-saas-new-relic-tutorial/ic797041.png "nastavení účtu")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="7d5fb-215">Na hello **aktivní uživatelé** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7d5fb-215">On hello **Active users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="7d5fb-216">![Aktivní uživatelé](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktivní uživatelé")</span><span class="sxs-lookup"><span data-stu-id="7d5fb-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="7d5fb-217">a.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-217">a.</span></span> <span data-ttu-id="7d5fb-218">V hello **e-mailu** textovému poli, hello typ e-mailová adresa platného uživatele Azure Active Directory chcete tooprovision.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-218">In hello **Email** textbox, type hello email address of a valid Azure Active Directory user you want tooprovision.</span></span>

    <span data-ttu-id="7d5fb-219">b.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-219">b.</span></span> <span data-ttu-id="7d5fb-220">Jako **Role** vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="7d5fb-221">c.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-221">c.</span></span> <span data-ttu-id="7d5fb-222">Klikněte na tlačítko **přidejte tohoto uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="7d5fb-223">Můžete použít všechny ostatní New Relic uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované New Relic tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-223">You can use any other New Relic user account creation tools or APIs provided by New Relic tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7d5fb-224">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7d5fb-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7d5fb-225">V této části povolíte tak, že udělíte přístup tooNew Relic toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNew Relic.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7d5fb-227">**tooassign Britta Simon tooNew New Relic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d5fb-227">**tooassign Britta Simon tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d5fb-228">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7d5fb-230">V seznamu aplikace hello vyberte **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-230">In hello applications list, select **New Relic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="7d5fb-232">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7d5fb-234">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-234">Click **Add** button.</span></span> <span data-ttu-id="7d5fb-235">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7d5fb-237">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7d5fb-238">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7d5fb-239">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7d5fb-240">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d5fb-240">Testing single sign-on</span></span>

<span data-ttu-id="7d5fb-241">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-241">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7d5fb-242">Když kliknete na dlaždici New Relic hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour aplikací New Relic.</span><span class="sxs-lookup"><span data-stu-id="7d5fb-242">When you click hello New Relic tile in hello Access Panel, you should get automatically signed-on tooyour New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d5fb-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7d5fb-243">Additional resources</span></span>

* [<span data-ttu-id="7d5fb-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d5fb-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d5fb-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d5fb-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

