---
title: "Kurz: Integrace Azure Active Directory se základním kamenem OnDemand | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a základním kamenem OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 85fd6eb4e93010d8f7477df236403e205e9f2d83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="477b0-103">Kurz: Integrace Azure Active Directory se základním kamenem OnDemand</span><span class="sxs-lookup"><span data-stu-id="477b0-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="477b0-104">V tomto kurzu zjistíte, jak toointegrate základním kamenem OnDemand službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="477b0-104">In this tutorial, you learn how toointegrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="477b0-105">Integrace základním kamenem OnDemand s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="477b0-105">Integrating Cornerstone OnDemand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="477b0-106">Můžete řídit ve službě Azure AD, který má přístup tooCornerstone OnDemand</span><span class="sxs-lookup"><span data-stu-id="477b0-106">You can control in Azure AD who has access tooCornerstone OnDemand</span></span>
- <span data-ttu-id="477b0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooCornerstone OnDemand (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="477b0-107">You can enable your users tooautomatically get signed-on tooCornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="477b0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="477b0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="477b0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="477b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="477b0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="477b0-110">Prerequisites</span></span>

<span data-ttu-id="477b0-111">tooconfigure integrace Azure AD se základním kamenem OnDemand, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="477b0-111">tooconfigure Azure AD integration with Cornerstone OnDemand, you need hello following items:</span></span>

- <span data-ttu-id="477b0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="477b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="477b0-113">Základním kamenem OnDemand jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="477b0-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="477b0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="477b0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="477b0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="477b0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="477b0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="477b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="477b0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="477b0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="477b0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="477b0-118">Scenario description</span></span>
<span data-ttu-id="477b0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="477b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="477b0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="477b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="477b0-121">Přidání základním kamenem OnDemand z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="477b0-121">Adding Cornerstone OnDemand from hello gallery</span></span>
2. <span data-ttu-id="477b0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="477b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-hello-gallery"></a><span data-ttu-id="477b0-123">Přidání základním kamenem OnDemand z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="477b0-123">Adding Cornerstone OnDemand from hello gallery</span></span>
<span data-ttu-id="477b0-124">tooconfigure hello integrace základním kamenem OnDemand do Azure AD, je nutné tooadd základním kamenem OnDemand hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="477b0-124">tooconfigure hello integration of Cornerstone OnDemand into Azure AD, you need tooadd Cornerstone OnDemand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="477b0-125">**tooadd základním kamenem OnDemand z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="477b0-125">**tooadd Cornerstone OnDemand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="477b0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="477b0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="477b0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="477b0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="477b0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="477b0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="477b0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="477b0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="477b0-133">Hello vyhledávacího pole zadejte **základním kamenem OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="477b0-133">In hello search box, type **Cornerstone OnDemand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="477b0-135">Na panelu výsledků hello vyberte **základním kamenem OnDemand**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="477b0-135">In hello results panel, select **Cornerstone OnDemand**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="477b0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="477b0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="477b0-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se základním kamenem OnDemand podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="477b0-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="477b0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v základním kamenem OnDemand je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="477b0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cornerstone OnDemand is tooa user in Azure AD.</span></span> <span data-ttu-id="477b0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v základním kamenem OnDemand musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="477b0-140">In other words, a link relationship between an Azure AD user and hello related user in Cornerstone OnDemand needs toobe established.</span></span>

<span data-ttu-id="477b0-141">V základním kamenem OnDemand přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="477b0-141">In Cornerstone OnDemand, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="477b0-142">tooconfigure a testování Azure AD jednotné přihlašování se základním kamenem OnDemand, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="477b0-142">tooconfigure and test Azure AD single sign-on with Cornerstone OnDemand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="477b0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="477b0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="477b0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="477b0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="477b0-145">**[Vytvoření zkušebního uživatele základním kamenem OnDemand](#creating-a-cornerstone-ondemand-test-user)**  -toohave protějšek Britta Simon v základním kamenem OnDemand, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="477b0-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - toohave a counterpart of Britta Simon in Cornerstone OnDemand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="477b0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="477b0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="477b0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="477b0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="477b0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="477b0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="477b0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="477b0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="477b0-150">**tooconfigure Azure AD jednotné přihlašování se základním kamenem OnDemand, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="477b0-150">**tooconfigure Azure AD single sign-on with Cornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="477b0-151">V portálu Azure, na hello hello **základním kamenem OnDemand** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="477b0-151">In hello Azure portal, on hello **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="477b0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="477b0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="477b0-155">Na hello **základním kamenem OnDemand domény a adresy URL** část, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="477b0-155">On hello **Cornerstone OnDemand Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="477b0-157">a.</span><span class="sxs-lookup"><span data-stu-id="477b0-157">a.</span></span> <span data-ttu-id="477b0-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="477b0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="477b0-159">b.</span><span class="sxs-lookup"><span data-stu-id="477b0-159">b.</span></span> <span data-ttu-id="477b0-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="477b0-160">In **Identifier** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="477b0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="477b0-161">These values are not real.</span></span> <span data-ttu-id="477b0-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="477b0-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="477b0-163">Obraťte se na [tým podpory základním kamenem OnDemand klienta](mailTo:moreinfo@csod.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="477b0-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="477b0-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="477b0-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="477b0-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="477b0-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="477b0-168">Na hello **základním kamenem OnDemand konfigurace** klikněte na tlačítko **konfigurace OnDemand základním kamenem** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="477b0-168">On hello **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="477b0-169">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="477b0-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="477b0-171">tooconfigure jednotného přihlašování na **základním kamenem OnDemand** straně, je nutné stáhnout hello toosend **certifikát**, **Sign-Out URL** a **jeden SAML Přihlášení adresa URL služby** příliš[tým podpory základním kamenem OnDemand](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="477b0-171">tooconfigure single sign-on on **Cornerstone OnDemand** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  too[Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="477b0-172">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="477b0-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="477b0-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="477b0-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="477b0-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="477b0-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="477b0-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="477b0-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="477b0-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="477b0-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="477b0-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="477b0-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="477b0-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="477b0-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="477b0-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="477b0-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="477b0-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="477b0-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="477b0-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="477b0-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="477b0-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="477b0-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="477b0-188">a.</span><span class="sxs-lookup"><span data-stu-id="477b0-188">a.</span></span> <span data-ttu-id="477b0-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="477b0-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="477b0-190">b.</span><span class="sxs-lookup"><span data-stu-id="477b0-190">b.</span></span> <span data-ttu-id="477b0-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="477b0-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="477b0-192">c.</span><span class="sxs-lookup"><span data-stu-id="477b0-192">c.</span></span> <span data-ttu-id="477b0-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="477b0-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="477b0-194">d.</span><span class="sxs-lookup"><span data-stu-id="477b0-194">d.</span></span> <span data-ttu-id="477b0-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="477b0-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="477b0-196">Vytvoření zkušebního uživatele základním kamenem OnDemand</span><span class="sxs-lookup"><span data-stu-id="477b0-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="477b0-197">V pořadí tooenable Azure AD Uživatelé toolog do základním kamenem OnDemand musí být zřízená do základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="477b0-197">In order tooenable Azure AD users toolog into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="477b0-198">V případě hello základním kamenem OnDemand zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="477b0-198">In hello case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="477b0-199">tooconfigure zřizování uživatelů, odesílání hello informace (například: název, e-mailu) o hello Azure AD uživatele chcete tooprovision toohello [tým podpory základním kamenem OnDemand](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="477b0-199">tooconfigure user provisioning, send hello information (e.g.: Name, Email) about hello Azure AD user you want tooprovision toohello [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="477b0-200">Můžete použít všechny ostatní základním kamenem OnDemand uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované základním kamenem OnDemand tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="477b0-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="477b0-201">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="477b0-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="477b0-202">V této části povolíte tak, že udělíte přístup tooCornerstone OnDemand Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="477b0-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCornerstone OnDemand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="477b0-204">**tooassign tooCornerstone Britta Simon OnDemand, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="477b0-204">**tooassign Britta Simon tooCornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="477b0-205">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="477b0-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="477b0-207">V seznamu aplikace hello vyberte **základním kamenem OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="477b0-207">In hello applications list, select **Cornerstone OnDemand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="477b0-209">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="477b0-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="477b0-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="477b0-211">Click **Add** button.</span></span> <span data-ttu-id="477b0-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="477b0-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="477b0-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="477b0-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="477b0-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="477b0-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="477b0-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="477b0-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="477b0-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="477b0-217">Testing single sign-on</span></span>

<span data-ttu-id="477b0-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="477b0-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="477b0-219">Když kliknete na dlaždici základním kamenem OnDemand hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour základním kamenem OnDemand aplikace.</span><span class="sxs-lookup"><span data-stu-id="477b0-219">When you click hello Cornerstone OnDemand tile in hello Access Panel, you should get automatically signed-on tooyour Cornerstone OnDemand application.</span></span>
<span data-ttu-id="477b0-220">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="477b0-220">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="477b0-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="477b0-221">Additional resources</span></span>

* [<span data-ttu-id="477b0-222">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="477b0-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="477b0-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="477b0-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

