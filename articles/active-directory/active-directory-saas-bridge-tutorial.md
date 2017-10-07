---
title: 'Kurz: Azure Active Directory integrace s most | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a most."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dbb6499-80c1-4d00-a0b4-e0ad5522cf0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 85e1917de63356a86aa87a0f82621391733ab2aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bridge"></a><span data-ttu-id="3234b-103">Kurz: Azure Active Directory integrace s most</span><span class="sxs-lookup"><span data-stu-id="3234b-103">Tutorial: Azure Active Directory integration with Bridge</span></span>

<span data-ttu-id="3234b-104">V tomto kurzu zjistíte, jak toointegrate přemostění službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3234b-104">In this tutorial, you learn how toointegrate Bridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3234b-105">Integrace most s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3234b-105">Integrating Bridge with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3234b-106">Můžete řídit ve službě Azure AD, který má přístup tooBridge</span><span class="sxs-lookup"><span data-stu-id="3234b-106">You can control in Azure AD who has access tooBridge</span></span>
- <span data-ttu-id="3234b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBridge (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3234b-107">You can enable your users tooautomatically get signed-on tooBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3234b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3234b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3234b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3234b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3234b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3234b-110">Prerequisites</span></span>

<span data-ttu-id="3234b-111">Integrace služby Azure AD s most tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3234b-111">tooconfigure Azure AD integration with Bridge, you need hello following items:</span></span>

- <span data-ttu-id="3234b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3234b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3234b-113">Most jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3234b-113">A Bridge single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3234b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3234b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3234b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3234b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3234b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3234b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3234b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3234b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3234b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3234b-118">Scenario description</span></span>
<span data-ttu-id="3234b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3234b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3234b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3234b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3234b-121">Přidání most z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3234b-121">Adding Bridge from hello gallery</span></span>
2. <span data-ttu-id="3234b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3234b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bridge-from-hello-gallery"></a><span data-ttu-id="3234b-123">Přidání most z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3234b-123">Adding Bridge from hello gallery</span></span>
<span data-ttu-id="3234b-124">tooconfigure hello integrace most do Azure AD, musíte tooadd most hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3234b-124">tooconfigure hello integration of Bridge into Azure AD, you need tooadd Bridge from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3234b-125">**Most tooadd z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3234b-125">**tooadd Bridge from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3234b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3234b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3234b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3234b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3234b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3234b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3234b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3234b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3234b-133">Hello vyhledávacího pole zadejte **most**.</span><span class="sxs-lookup"><span data-stu-id="3234b-133">In hello search box, type **Bridge**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_search.png)

5. <span data-ttu-id="3234b-135">Na panelu výsledků hello vyberte **most**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3234b-135">In hello results panel, select **Bridge**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3234b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3234b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3234b-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s most podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3234b-138">In this section, you configure and test Azure AD single sign-on with Bridge based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3234b-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v most je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3234b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bridge is tooa user in Azure AD.</span></span> <span data-ttu-id="3234b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v most musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3234b-140">In other words, a link relationship between an Azure AD user and hello related user in Bridge needs toobe established.</span></span>

<span data-ttu-id="3234b-141">V mostu, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3234b-141">In Bridge, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3234b-142">tooconfigure a testu Azure AD jednotné přihlašování s most, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3234b-142">tooconfigure and test Azure AD single sign-on with Bridge, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3234b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3234b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3234b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3234b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3234b-145">**[Vytvoření zkušebního uživatele most](#creating-a-bridge-test-user)**  -toohave protějšek Britta Simon v mostu, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3234b-145">**[Creating a Bridge test user](#creating-a-bridge-test-user)** - toohave a counterpart of Britta Simon in Bridge that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3234b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3234b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3234b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3234b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3234b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3234b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3234b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci most.</span><span class="sxs-lookup"><span data-stu-id="3234b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bridge application.</span></span>

<span data-ttu-id="3234b-150">**tooconfigure Azure AD jednotné přihlašování s most, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3234b-150">**tooconfigure Azure AD single sign-on with Bridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="3234b-151">V portálu Azure, na hello hello **most** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3234b-151">In hello Azure portal, on hello **Bridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3234b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3234b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_samlbase.png)

3. <span data-ttu-id="3234b-155">Na hello **most domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3234b-155">On hello **Bridge Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_url.png)

    <span data-ttu-id="3234b-157">a.</span><span class="sxs-lookup"><span data-stu-id="3234b-157">a.</span></span> <span data-ttu-id="3234b-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="3234b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.bridgeapp.com`</span></span>

    <span data-ttu-id="3234b-159">b.</span><span class="sxs-lookup"><span data-stu-id="3234b-159">b.</span></span> <span data-ttu-id="3234b-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.bridgeapp.com`</span><span class="sxs-lookup"><span data-stu-id="3234b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.bridgeapp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3234b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3234b-161">These values are not real.</span></span> <span data-ttu-id="3234b-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3234b-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3234b-163">Obraťte se na [tým podpory most klienta](https://community.bridgeapp.com/community/help) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3234b-163">Contact [Bridge Client support team](https://community.bridgeapp.com/community/help) tooget these values.</span></span> 
 
4. <span data-ttu-id="3234b-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3234b-164">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_certificate.png) 

5. <span data-ttu-id="3234b-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3234b-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3234b-168">Na hello **konfigurace mostu** klikněte na tlačítko **konfigurace mostu** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3234b-168">On hello **Bridge Configuration** section, click **Configure Bridge** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3234b-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3234b-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_configure.png) 

7. <span data-ttu-id="3234b-171">tooconfigure jednotného přihlašování na **most** straně, je nutné stáhnout hello toosend **Certificate(Raw)** a **SAML Entity ID, SAML jeden přihlašování adresa URL služby a Sign-Out URL**příliš[tým podpory most](https://community.bridgeapp.com/community/help).</span><span class="sxs-lookup"><span data-stu-id="3234b-171">tooconfigure single sign-on on **Bridge** side, you need toosend hello downloaded **Certificate(Raw)** and **SAML Entity ID, SAML Single Sign-On Service URL, and Sign-Out URL** too[Bridge support team](https://community.bridgeapp.com/community/help).</span></span> 

> [!TIP]
> <span data-ttu-id="3234b-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3234b-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3234b-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3234b-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3234b-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3234b-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3234b-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3234b-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="3234b-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3234b-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3234b-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3234b-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3234b-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3234b-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3234b-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3234b-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3234b-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3234b-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3234b-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3234b-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3234b-187">a.</span><span class="sxs-lookup"><span data-stu-id="3234b-187">a.</span></span> <span data-ttu-id="3234b-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3234b-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3234b-189">b.</span><span class="sxs-lookup"><span data-stu-id="3234b-189">b.</span></span> <span data-ttu-id="3234b-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3234b-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3234b-191">c.</span><span class="sxs-lookup"><span data-stu-id="3234b-191">c.</span></span> <span data-ttu-id="3234b-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3234b-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3234b-193">d.</span><span class="sxs-lookup"><span data-stu-id="3234b-193">d.</span></span> <span data-ttu-id="3234b-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3234b-194">Click **Create**.</span></span>
 
### <a name="creating-a-bridge-test-user"></a><span data-ttu-id="3234b-195">Vytvoření zkušebního uživatele most</span><span class="sxs-lookup"><span data-stu-id="3234b-195">Creating a Bridge test user</span></span>

<span data-ttu-id="3234b-196">V této části vytvoříte uživatele volal Britta Simon v most.</span><span class="sxs-lookup"><span data-stu-id="3234b-196">In this section, you create a user called Britta Simon in Bridge.</span></span> <span data-ttu-id="3234b-197">Práce s [tým podpory most klienta](https://community.bridgeapp.com/community/help) toocreate uživatele v platformě hello.</span><span class="sxs-lookup"><span data-stu-id="3234b-197">Work with [Bridge Client support team](https://community.bridgeapp.com/community/help) toocreate a user in hello platform.</span></span> <span data-ttu-id="3234b-198">Můžete zvýšit lístku podpory hello s most z <a href="https://community.bridgeapp.com/community/help">sem</a> tooadd hello uživatelé v platformě most hello.</span><span class="sxs-lookup"><span data-stu-id="3234b-198">You can raise hello support ticket with Bridge from <a href="https://community.bridgeapp.com/community/help">here</a> tooadd hello users in hello Bridge platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3234b-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3234b-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3234b-200">V této části povolíte tak, že udělíte přístup tooBridge toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3234b-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBridge.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3234b-202">**tooassign Britta Simon tooBridge, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3234b-202">**tooassign Britta Simon tooBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="3234b-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3234b-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3234b-205">V seznamu aplikace hello vyberte **most**.</span><span class="sxs-lookup"><span data-stu-id="3234b-205">In hello applications list, select **Bridge**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_app.png) 

3. <span data-ttu-id="3234b-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3234b-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3234b-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3234b-209">Click **Add** button.</span></span> <span data-ttu-id="3234b-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3234b-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3234b-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3234b-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3234b-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3234b-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3234b-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3234b-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3234b-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3234b-215">Testing single sign-on</span></span>

<span data-ttu-id="3234b-216">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3234b-216">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="3234b-217">Když kliknete na dlaždici most hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour most aplikace.</span><span class="sxs-lookup"><span data-stu-id="3234b-217">When you click hello Bridge tile in hello Access Panel, you should get automatically signed-on tooyour Bridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3234b-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3234b-218">Additional resources</span></span>

* [<span data-ttu-id="3234b-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3234b-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3234b-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3234b-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_203.png

