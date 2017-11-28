---
title: 'Kurz: Azure Active Directory integrace s Aravo | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Aravo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 224939d8-2c9c-4561-968d-62722f5ab5ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 8f1336fa307fa9e8d625440a573d9f9d79dd820b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aravo"></a><span data-ttu-id="48395-103">Kurz: Azure Active Directory integrace s Aravo</span><span class="sxs-lookup"><span data-stu-id="48395-103">Tutorial: Azure Active Directory integration with Aravo</span></span>

<span data-ttu-id="48395-104">V tomto kurzu zjistíte, jak toointegrate Aravo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48395-104">In this tutorial, you learn how toointegrate Aravo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48395-105">Integrace Aravo s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="48395-105">Integrating Aravo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="48395-106">Můžete řídit ve službě Azure AD, který má přístup tooAravo</span><span class="sxs-lookup"><span data-stu-id="48395-106">You can control in Azure AD who has access tooAravo</span></span>
- <span data-ttu-id="48395-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAravo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="48395-107">You can enable your users tooautomatically get signed-on tooAravo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48395-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="48395-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="48395-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48395-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48395-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="48395-110">Prerequisites</span></span>

<span data-ttu-id="48395-111">Integrace služby Azure AD s Aravo tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="48395-111">tooconfigure Azure AD integration with Aravo, you need hello following items:</span></span>

- <span data-ttu-id="48395-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="48395-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48395-113">Aravo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="48395-113">An Aravo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48395-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="48395-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48395-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="48395-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48395-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="48395-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48395-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48395-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48395-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="48395-118">Scenario description</span></span>
<span data-ttu-id="48395-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="48395-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48395-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="48395-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48395-121">Přidání Aravo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="48395-121">Adding Aravo from hello gallery</span></span>
2. <span data-ttu-id="48395-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48395-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aravo-from-hello-gallery"></a><span data-ttu-id="48395-123">Přidání Aravo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="48395-123">Adding Aravo from hello gallery</span></span>
<span data-ttu-id="48395-124">tooconfigure hello integrace Aravo do Azure AD, je nutné tooadd Aravo hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="48395-124">tooconfigure hello integration of Aravo into Azure AD, you need tooadd Aravo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="48395-125">**tooadd Aravo z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48395-125">**tooadd Aravo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="48395-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48395-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48395-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="48395-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="48395-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48395-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="48395-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48395-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="48395-133">Hello vyhledávacího pole zadejte **Aravo**.</span><span class="sxs-lookup"><span data-stu-id="48395-133">In hello search box, type **Aravo**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_search.png)

5. <span data-ttu-id="48395-135">Na panelu výsledků hello vyberte **Aravo**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="48395-135">In hello results panel, select **Aravo**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48395-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="48395-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48395-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Aravo podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="48395-138">In this section, you configure and test Azure AD single sign-on with Aravo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48395-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Aravo je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48395-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Aravo is tooa user in Azure AD.</span></span> <span data-ttu-id="48395-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Aravo musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="48395-140">In other words, a link relationship between an Azure AD user and hello related user in Aravo needs toobe established.</span></span>

<span data-ttu-id="48395-141">V Aravo, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="48395-141">In Aravo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="48395-142">tooconfigure a testu Azure AD jednotné přihlašování s Aravo, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="48395-142">tooconfigure and test Azure AD single sign-on with Aravo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="48395-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="48395-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="48395-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48395-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48395-145">**[Vytváření testovacího uživatele Aravo](#creating-an-aravo-test-user)**  -toohave protějšek Britta Simon v Aravo, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="48395-145">**[Creating an Aravo test user](#creating-an-aravo-test-user)** - toohave a counterpart of Britta Simon in Aravo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="48395-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48395-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48395-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="48395-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48395-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48395-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48395-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Aravo.</span><span class="sxs-lookup"><span data-stu-id="48395-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Aravo application.</span></span>

<span data-ttu-id="48395-150">**tooconfigure Azure AD jednotné přihlašování s Aravo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48395-150">**tooconfigure Azure AD single sign-on with Aravo, perform hello following steps:**</span></span>

1. <span data-ttu-id="48395-151">V portálu Azure, na hello hello **Aravo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="48395-151">In hello Azure portal, on hello **Aravo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="48395-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48395-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_samlbase.png)

3. <span data-ttu-id="48395-155">Na hello **Aravo domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="48395-155">On hello **Aravo Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_url.png)

    <span data-ttu-id="48395-157">a.</span><span class="sxs-lookup"><span data-stu-id="48395-157">a.</span></span> <span data-ttu-id="48395-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.aravo.com`</span><span class="sxs-lookup"><span data-stu-id="48395-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.aravo.com`</span></span>

    <span data-ttu-id="48395-159">b.</span><span class="sxs-lookup"><span data-stu-id="48395-159">b.</span></span> <span data-ttu-id="48395-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.aravo.com/aems/login.do`</span><span class="sxs-lookup"><span data-stu-id="48395-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.aravo.com/aems/login.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48395-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="48395-161">These values are not real.</span></span> <span data-ttu-id="48395-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="48395-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="48395-163">Obraťte se na [tým podpory Aravo](http://www.aravo.com/about-us/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="48395-163">Contact [Aravo support team](http://www.aravo.com/about-us/contact/) tooget these values.</span></span>
 
4. <span data-ttu-id="48395-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="48395-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_certificate.png) 

5. <span data-ttu-id="48395-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48395-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48395-168">Na hello **Aravo konfigurace** klikněte na tlačítko **konfigurace Aravo** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="48395-168">On hello **Aravo Configuration** section, click **Configure Aravo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="48395-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="48395-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_configure.png) 

7. <span data-ttu-id="48395-171">tooconfigure jednotného přihlašování na **Aravo** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**příliš[tým podpory Aravo](http://www.aravo.com/about-us/contact/).</span><span class="sxs-lookup"><span data-stu-id="48395-171">tooconfigure single sign-on on **Aravo** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Aravo support team](http://www.aravo.com/about-us/contact/).</span></span> 


> [!TIP]
> <span data-ttu-id="48395-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="48395-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="48395-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="48395-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="48395-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48395-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48395-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="48395-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="48395-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="48395-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="48395-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48395-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="48395-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="48395-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48395-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="48395-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48395-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="48395-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48395-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="48395-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-aravo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48395-187">a.</span><span class="sxs-lookup"><span data-stu-id="48395-187">a.</span></span> <span data-ttu-id="48395-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48395-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48395-189">b.</span><span class="sxs-lookup"><span data-stu-id="48395-189">b.</span></span> <span data-ttu-id="48395-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48395-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48395-191">c.</span><span class="sxs-lookup"><span data-stu-id="48395-191">c.</span></span> <span data-ttu-id="48395-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="48395-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="48395-193">d.</span><span class="sxs-lookup"><span data-stu-id="48395-193">d.</span></span> <span data-ttu-id="48395-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="48395-194">Click **Create**.</span></span>
 
### <a name="creating-an-aravo-test-user"></a><span data-ttu-id="48395-195">Vytváření testovacího uživatele Aravo</span><span class="sxs-lookup"><span data-stu-id="48395-195">Creating an Aravo test user</span></span>

<span data-ttu-id="48395-196">Hello cílem této části je toocreate volal Britta Simon v Aravo uživatele.</span><span class="sxs-lookup"><span data-stu-id="48395-196">hello objective of this section is toocreate a user called Britta Simon in Aravo.</span></span> <span data-ttu-id="48395-197">Práce s [tým podpory Aravo](http://www.aravo.com/about-us/contact/) tooadd hello uživatele v hello Aravo účtu.</span><span class="sxs-lookup"><span data-stu-id="48395-197">Work with [Aravo support team](http://www.aravo.com/about-us/contact/) tooadd hello users in hello Aravo account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="48395-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="48395-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="48395-199">V této části povolíte tak, že udělíte přístup tooAravo toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="48395-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAravo.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="48395-201">**tooassign Britta Simon tooAravo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="48395-201">**tooassign Britta Simon tooAravo, perform hello following steps:**</span></span>

1. <span data-ttu-id="48395-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="48395-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="48395-204">V seznamu aplikace hello vyberte **Aravo**.</span><span class="sxs-lookup"><span data-stu-id="48395-204">In hello applications list, select **Aravo**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-aravo-tutorial/tutorial_aravo_app.png) 

3. <span data-ttu-id="48395-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="48395-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="48395-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="48395-208">Click **Add** button.</span></span> <span data-ttu-id="48395-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48395-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="48395-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="48395-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="48395-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48395-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48395-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="48395-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48395-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="48395-214">Testing single sign-on</span></span>

<span data-ttu-id="48395-215">Hello cílem této části je tootest vaší Microsoft Azure AD Single Sign-On konfigurace pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="48395-215">hello objective of this section is tootest your Microsoft Azure AD Single Sign-On configuration using hello Access Panel.</span></span>

<span data-ttu-id="48395-216">Když kliknete na dlaždici Aravo hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Aravo aplikace.</span><span class="sxs-lookup"><span data-stu-id="48395-216">When you click hello Aravo tile in hello Access Panel, you should get automatically signed-on tooyour Aravo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48395-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48395-217">Additional resources</span></span>

* [<span data-ttu-id="48395-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48395-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48395-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48395-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aravo-tutorial/tutorial_general_203.png

