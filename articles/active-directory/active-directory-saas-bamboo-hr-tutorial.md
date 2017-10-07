---
title: 'Kurz: Azure Active Directory integrace s BambooHR | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a BambooHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: f9083f846beb3a4bf4cebbf18b42aba2dfef2472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="a1518-103">Kurz: Azure Active Directory integrace s BambooHR</span><span class="sxs-lookup"><span data-stu-id="a1518-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="a1518-104">V tomto kurzu zjistíte, jak toointegrate BambooHR s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a1518-104">In this tutorial, you learn how toointegrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a1518-105">Integrace BambooHR s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a1518-105">Integrating BambooHR with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a1518-106">Můžete řídit ve službě Azure AD, který má přístup tooBambooHR</span><span class="sxs-lookup"><span data-stu-id="a1518-106">You can control in Azure AD who has access tooBambooHR</span></span>
- <span data-ttu-id="a1518-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBambooHR (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1518-107">You can enable your users tooautomatically get signed-on tooBambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a1518-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a1518-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a1518-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a1518-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1518-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a1518-110">Prerequisites</span></span>

<span data-ttu-id="a1518-111">Integrace služby Azure AD s BambooHR tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a1518-111">tooconfigure Azure AD integration with BambooHR, you need hello following items:</span></span>

- <span data-ttu-id="a1518-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1518-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a1518-113">BambooHR jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a1518-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a1518-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a1518-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a1518-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a1518-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a1518-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a1518-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a1518-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a1518-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a1518-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a1518-118">Scenario description</span></span>
<span data-ttu-id="a1518-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a1518-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a1518-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a1518-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a1518-121">Přidání BambooHR z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a1518-121">Adding BambooHR from hello gallery</span></span>
2. <span data-ttu-id="a1518-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1518-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-hello-gallery"></a><span data-ttu-id="a1518-123">Přidání BambooHR z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a1518-123">Adding BambooHR from hello gallery</span></span>
<span data-ttu-id="a1518-124">tooconfigure hello integrace BambooHR do Azure AD, je nutné tooadd BambooHR hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a1518-124">tooconfigure hello integration of BambooHR into Azure AD, you need tooadd BambooHR from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a1518-125">**tooadd BambooHR z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a1518-125">**tooadd BambooHR from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a1518-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a1518-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a1518-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a1518-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a1518-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a1518-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a1518-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1518-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a1518-133">Hello vyhledávacího pole zadejte **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="a1518-133">In hello search box, type **BambooHR**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="a1518-135">Na panelu výsledků hello vyberte **BambooHR**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-135">In hello results panel, select **BambooHR**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a1518-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1518-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a1518-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BambooHR podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a1518-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a1518-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v BambooHR je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1518-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BambooHR is tooa user in Azure AD.</span></span> <span data-ttu-id="a1518-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v BambooHR musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a1518-140">In other words, a link relationship between an Azure AD user and hello related user in BambooHR needs toobe established.</span></span>

<span data-ttu-id="a1518-141">V BambooHR, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a1518-141">In BambooHR, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a1518-142">tooconfigure a testu Azure AD jednotné přihlašování s BambooHR, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a1518-142">tooconfigure and test Azure AD single sign-on with BambooHR, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a1518-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a1518-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a1518-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a1518-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a1518-145">**[Vytvoření zkušebního uživatele BambooHR](#creating-a-bamboohr-test-user)**  -toohave protějšek Britta Simon v BambooHR, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a1518-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - toohave a counterpart of Britta Simon in BambooHR that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a1518-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1518-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a1518-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a1518-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a1518-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1518-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a1518-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci BambooHR.</span><span class="sxs-lookup"><span data-stu-id="a1518-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="a1518-150">**tooconfigure Azure AD jednotné přihlašování s BambooHR, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a1518-150">**tooconfigure Azure AD single sign-on with BambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="a1518-151">V portálu Azure, na hello hello **BambooHR** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a1518-151">In hello Azure portal, on hello **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a1518-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1518-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="a1518-155">Na hello **BambooHR domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a1518-155">On hello **BambooHR Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="a1518-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="a1518-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="a1518-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a1518-158">This value is not real.</span></span> <span data-ttu-id="a1518-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1518-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a1518-160">Obraťte se na [tým podpory BambooHR klienta](https://www.bamboohr.com/contact.php) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a1518-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) tooget this value.</span></span> 
 
4. <span data-ttu-id="a1518-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a1518-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="a1518-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1518-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a1518-165">Na hello **BambooHR konfigurace** klikněte na tlačítko **konfigurace BambooHR** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a1518-165">On hello **BambooHR Configuration** section, click **Configure BambooHR** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a1518-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a1518-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="a1518-168">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti BambooHR.</span><span class="sxs-lookup"><span data-stu-id="a1518-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="a1518-169">Na domovskou stránku hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a1518-169">On hello homepage, perform hello following steps:</span></span>
   
    <span data-ttu-id="a1518-170">![Jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="a1518-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="a1518-171">a.</span><span class="sxs-lookup"><span data-stu-id="a1518-171">a.</span></span> <span data-ttu-id="a1518-172">Klikněte na tlačítko **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a1518-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="a1518-173">b.</span><span class="sxs-lookup"><span data-stu-id="a1518-173">b.</span></span> <span data-ttu-id="a1518-174">V nabídce aplikace hello na levé straně hello, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a1518-174">In hello apps menu on hello left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="a1518-175">c.</span><span class="sxs-lookup"><span data-stu-id="a1518-175">c.</span></span> <span data-ttu-id="a1518-176">Klikněte na tlačítko **SAML Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="a1518-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="a1518-177">V hello **SAML Single Sign-On** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a1518-177">In hello **SAML Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a1518-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="a1518-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="a1518-179">a.</span><span class="sxs-lookup"><span data-stu-id="a1518-179">a.</span></span> <span data-ttu-id="a1518-180">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu do hello **adresu Url pro přihlášení SSO** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a1518-180">Paste hello **SAML Single Sign-On Service URL** value into hello **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="a1518-181">b.</span><span class="sxs-lookup"><span data-stu-id="a1518-181">b.</span></span> <span data-ttu-id="a1518-182">Otevřete kódování base-64 kódovaného certifikátu si stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="a1518-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="a1518-183">c.</span><span class="sxs-lookup"><span data-stu-id="a1518-183">c.</span></span> <span data-ttu-id="a1518-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a1518-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="a1518-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a1518-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a1518-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a1518-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a1518-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a1518-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a1518-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="a1518-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a1518-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a1518-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a1518-192">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a1518-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a1518-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a1518-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a1518-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a1518-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a1518-198">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a1518-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a1518-200">a.</span><span class="sxs-lookup"><span data-stu-id="a1518-200">a.</span></span> <span data-ttu-id="a1518-201">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a1518-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a1518-202">b.</span><span class="sxs-lookup"><span data-stu-id="a1518-202">b.</span></span> <span data-ttu-id="a1518-203">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a1518-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a1518-204">c.</span><span class="sxs-lookup"><span data-stu-id="a1518-204">c.</span></span> <span data-ttu-id="a1518-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a1518-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a1518-206">d.</span><span class="sxs-lookup"><span data-stu-id="a1518-206">d.</span></span> <span data-ttu-id="a1518-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a1518-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="a1518-208">Vytvoření zkušebního uživatele BambooHR</span><span class="sxs-lookup"><span data-stu-id="a1518-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="a1518-209">Uživatelé toolog tooenable Azure AD v tooBambooHR, se musí být zřízená do BambooHR.</span><span class="sxs-lookup"><span data-stu-id="a1518-209">tooenable Azure AD users toolog in tooBambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="a1518-210">V případě hello BambooHR zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a1518-210">In hello case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="a1518-211">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a1518-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="a1518-212">Přihlaste se tooyour **BambooHR** lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="a1518-212">Log in tooyour **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="a1518-213">V panelu nástrojů hello hello nahoře, klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a1518-213">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="a1518-214">![Nastavení](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="a1518-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="a1518-215">Klikněte na tlačítko **přehled**.</span><span class="sxs-lookup"><span data-stu-id="a1518-215">Click **Overview**.</span></span>

4. <span data-ttu-id="a1518-216">V levém navigačním podokně hello přejděte příliš**zabezpečení \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a1518-216">In hello left navigation pane, go too**Security \> Users**.</span></span>

5. <span data-ttu-id="a1518-217">Typ hello uživatelské jméno, heslo a e-mailovou adresu platného účtu AAD, že chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="a1518-217">Type hello user name, password, and email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

6. <span data-ttu-id="a1518-218">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a1518-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="a1518-219">Můžete použít všechny ostatní BambooHR uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované BambooHR tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a1518-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a1518-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a1518-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a1518-221">V této části povolíte tak, že udělíte přístup tooBambooHR toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a1518-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBambooHR.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a1518-223">**tooassign Britta Simon tooBambooHR, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a1518-223">**tooassign Britta Simon tooBambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="a1518-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a1518-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a1518-226">V seznamu aplikace hello vyberte **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="a1518-226">In hello applications list, select **BambooHR**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="a1518-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a1518-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a1518-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a1518-230">Click **Add** button.</span></span> <span data-ttu-id="a1518-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1518-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a1518-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a1518-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a1518-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1518-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a1518-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a1518-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a1518-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a1518-236">Testing single sign-on</span></span>

<span data-ttu-id="a1518-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a1518-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a1518-238">Když kliknete na dlaždici BambooHR hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour BambooHR aplikace.</span><span class="sxs-lookup"><span data-stu-id="a1518-238">When you click hello BambooHR tile in hello Access Panel, you should get automatically signed-on tooyour BambooHR application.</span></span>
<span data-ttu-id="a1518-239">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a1518-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a1518-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a1518-240">Additional resources</span></span>

* [<span data-ttu-id="a1518-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a1518-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a1518-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a1518-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

