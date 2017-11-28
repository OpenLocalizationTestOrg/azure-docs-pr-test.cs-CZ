---
title: 'Kurz: Azure Active Directory integrace s 23 Video | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 23 Video."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 3430e4db3cd1114db62233e6699618071a3646ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="944bf-103">Kurz: Azure Active Directory integrace s 23 Video</span><span class="sxs-lookup"><span data-stu-id="944bf-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="944bf-104">V tomto kurzu zjistíte, jak toointegrate 23 Video s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="944bf-104">In this tutorial, you learn how toointegrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="944bf-105">Integrace 23 Video s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="944bf-105">Integrating 23 Video with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="944bf-106">Můžete řídit ve službě Azure AD, který má přístup too23 Video</span><span class="sxs-lookup"><span data-stu-id="944bf-106">You can control in Azure AD who has access too23 Video</span></span>
- <span data-ttu-id="944bf-107">Můžete povolit uživatelům získat tooautomatically přihlášeného too23 Video (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="944bf-107">You can enable your users tooautomatically get signed-on too23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="944bf-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="944bf-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="944bf-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="944bf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="944bf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="944bf-110">Prerequisites</span></span>

<span data-ttu-id="944bf-111">tooconfigure integrace Azure AD s 23 Video, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="944bf-111">tooconfigure Azure AD integration with 23 Video, you need hello following items:</span></span>

- <span data-ttu-id="944bf-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="944bf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="944bf-113">Předplatné povolené 23 Video jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="944bf-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="944bf-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="944bf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="944bf-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="944bf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="944bf-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="944bf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="944bf-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="944bf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="944bf-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="944bf-118">Scenario description</span></span>
<span data-ttu-id="944bf-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="944bf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="944bf-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="944bf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="944bf-121">Přidání 23 Video z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="944bf-121">Adding 23 Video from hello gallery</span></span>
2. <span data-ttu-id="944bf-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="944bf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-hello-gallery"></a><span data-ttu-id="944bf-123">Přidání 23 Video z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="944bf-123">Adding 23 Video from hello gallery</span></span>
<span data-ttu-id="944bf-124">tooconfigure hello integrace 23 videa do Azure AD, je nutné tooadd 23 Video hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="944bf-124">tooconfigure hello integration of 23 Video into Azure AD, you need tooadd 23 Video from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="944bf-125">**Proveďte Video z Galerie hello tooadd 23 hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="944bf-125">**tooadd 23 Video from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="944bf-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="944bf-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="944bf-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="944bf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="944bf-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="944bf-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="944bf-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="944bf-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="944bf-133">Hello vyhledávacího pole zadejte **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="944bf-133">In hello search box, type **23 Video**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="944bf-135">Na panelu výsledků hello vyberte **23 Video**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="944bf-135">In hello results panel, select **23 Video**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="944bf-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="944bf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="944bf-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 23 Video podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="944bf-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="944bf-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v 23 Video je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="944bf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 23 Video is tooa user in Azure AD.</span></span> <span data-ttu-id="944bf-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello ve 23 Video musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="944bf-140">In other words, a link relationship between an Azure AD user and hello related user in 23 Video needs toobe established.</span></span>

<span data-ttu-id="944bf-141">V 23 Video přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="944bf-141">In 23 Video, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="944bf-142">tooconfigure a testu Azure AD jednotné přihlašování s 23 Video, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="944bf-142">tooconfigure and test Azure AD single sign-on with 23 Video, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="944bf-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="944bf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="944bf-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="944bf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="944bf-145">**[Vytvoření zkušebního uživatele 23 Video](#creating-a-23-video-test-user)**  -toohave protějšek Britta Simon ve 23 Video, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="944bf-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - toohave a counterpart of Britta Simon in 23 Video that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="944bf-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="944bf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="944bf-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="944bf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="944bf-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="944bf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="944bf-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 23 Video.</span><span class="sxs-lookup"><span data-stu-id="944bf-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="944bf-150">**tooconfigure Azure AD jednotné přihlašování s 23 Video, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="944bf-150">**tooconfigure Azure AD single sign-on with 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="944bf-151">V portálu Azure, na hello hello **23 Video** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="944bf-151">In hello Azure portal, on hello **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="944bf-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="944bf-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="944bf-155">Na hello **23 Video domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="944bf-155">On hello **23 Video Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="944bf-157">a.</span><span class="sxs-lookup"><span data-stu-id="944bf-157">a.</span></span> <span data-ttu-id="944bf-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="944bf-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="944bf-159">b.</span><span class="sxs-lookup"><span data-stu-id="944bf-159">b.</span></span> <span data-ttu-id="944bf-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="944bf-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="944bf-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="944bf-161">These values are not real.</span></span> <span data-ttu-id="944bf-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="944bf-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="944bf-163">Obraťte se na [23 tým podpory Video klienta](mailto:support@23company.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="944bf-163">Contact [23 Video Client support team](mailto:support@23company.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="944bf-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="944bf-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="944bf-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="944bf-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="944bf-168">Na hello **23 konfigurace Video** klikněte na tlačítko **konfigurace Video 23** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="944bf-168">On hello **23 Video Configuration** section, click **Configure 23 Video** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="944bf-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="944bf-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="944bf-171">tooconfigure jednotného přihlašování na **23 Video** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**příliš[23 tým podpory Video](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="944bf-171">tooconfigure single sign-on on **23 Video** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="944bf-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="944bf-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="944bf-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="944bf-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="944bf-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="944bf-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="944bf-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="944bf-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="944bf-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="944bf-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="944bf-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="944bf-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="944bf-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="944bf-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="944bf-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="944bf-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="944bf-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="944bf-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="944bf-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="944bf-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="944bf-187">a.</span><span class="sxs-lookup"><span data-stu-id="944bf-187">a.</span></span> <span data-ttu-id="944bf-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="944bf-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="944bf-189">b.</span><span class="sxs-lookup"><span data-stu-id="944bf-189">b.</span></span> <span data-ttu-id="944bf-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="944bf-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="944bf-191">c.</span><span class="sxs-lookup"><span data-stu-id="944bf-191">c.</span></span> <span data-ttu-id="944bf-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="944bf-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="944bf-193">d.</span><span class="sxs-lookup"><span data-stu-id="944bf-193">d.</span></span> <span data-ttu-id="944bf-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="944bf-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="944bf-195">Vytvoření zkušebního uživatele 23 Video</span><span class="sxs-lookup"><span data-stu-id="944bf-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="944bf-196">Hello cílem této části je toocreate uživatel volal Britta Simon v 23 Video.</span><span class="sxs-lookup"><span data-stu-id="944bf-196">hello objective of this section is toocreate a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="944bf-197">**toocreate uživatel volal Britta Simon v 23 Video, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="944bf-197">**toocreate a user called Britta Simon in 23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="944bf-198">Přihlaste se na tooyour 23 Video společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="944bf-198">Sign on tooyour 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="944bf-199">Přejděte příliš**nastavení**.</span><span class="sxs-lookup"><span data-stu-id="944bf-199">Go too**Settings**.</span></span>
 
3. <span data-ttu-id="944bf-200">V **uživatelé** klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="944bf-200">In **Users** section, click **Configure**.</span></span>
   
    ![Přiřadit uživatele][400]

4. <span data-ttu-id="944bf-202">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="944bf-202">Click **Add a new user**.</span></span> 
   
    ![Přiřadit uživatele][401]

5. <span data-ttu-id="944bf-204">V hello **pozvání uživatele toojoin tento web** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="944bf-204">In hello **Invite someone toojoin this site** section, perform hello following steps:</span></span>
   
    ![Přiřadit uživatele][402]

    <span data-ttu-id="944bf-206">a.</span><span class="sxs-lookup"><span data-stu-id="944bf-206">a.</span></span> <span data-ttu-id="944bf-207">V hello **e-mailové adresy** textovému poli, zadejte Britta Simon e-mailovou adresu ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="944bf-207">In hello **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="944bf-208">b.</span><span class="sxs-lookup"><span data-stu-id="944bf-208">b.</span></span> <span data-ttu-id="944bf-209">Klikněte na tlačítko **přidat hello uživatele**.</span><span class="sxs-lookup"><span data-stu-id="944bf-209">Click **Add hello user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="944bf-210">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="944bf-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="944bf-211">V této části povolíte Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup too23 Video.</span><span class="sxs-lookup"><span data-stu-id="944bf-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too23 Video.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="944bf-213">**tooassign Britta Simon too23 Video, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="944bf-213">**tooassign Britta Simon too23 Video, perform hello following steps:**</span></span>

1. <span data-ttu-id="944bf-214">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="944bf-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="944bf-216">V seznamu aplikace hello vyberte **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="944bf-216">In hello applications list, select **23 Video**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="944bf-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="944bf-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="944bf-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="944bf-220">Click **Add** button.</span></span> <span data-ttu-id="944bf-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="944bf-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="944bf-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="944bf-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="944bf-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="944bf-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="944bf-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="944bf-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="944bf-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="944bf-226">Testing single sign-on</span></span>

<span data-ttu-id="944bf-227">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="944bf-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="944bf-228">Když kliknete na dlaždici Video hello 23 v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour 23 Video aplikace.</span><span class="sxs-lookup"><span data-stu-id="944bf-228">When you click hello 23 Video tile in hello Access Panel, you should get automatically signed-on tooyour 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="944bf-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="944bf-229">Additional resources</span></span>

* [<span data-ttu-id="944bf-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="944bf-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="944bf-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="944bf-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
