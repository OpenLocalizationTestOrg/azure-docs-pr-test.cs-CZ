---
title: "Kurz: Azure Active Directory integrace s Learning stanici pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Learning stanici pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: dc08aa444b85f35a4458768ac560ec663baa1c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a><span data-ttu-id="02c08-103">Kurz: Azure Active Directory integrace s Learning stanici pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="02c08-103">Tutorial: Azure Active Directory integration with Learning Seat LMS</span></span>

<span data-ttu-id="02c08-104">V tomto kurzu zjistíte, jak toointegrate vzdělávacího procesu stanici Learning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="02c08-104">In this tutorial, you learn how toointegrate Learning Seat LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="02c08-105">Integrace vzdělávacího procesu stanici Learning s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="02c08-105">Integrating Learning Seat LMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="02c08-106">Můžete řídit ve službě Azure AD, který má přístup tooLearning stanici pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="02c08-106">You can control in Azure AD who has access tooLearning Seat LMS</span></span>
- <span data-ttu-id="02c08-107">Vaši uživatelé tooautomatically get přihlášeného tooLearning stanici pro správu vzdělávacího procesu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="02c08-107">You can enable your users tooautomatically get signed-on tooLearning Seat LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="02c08-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="02c08-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="02c08-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="02c08-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="02c08-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="02c08-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02c08-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="02c08-111">Prerequisites</span></span>

<span data-ttu-id="02c08-112">tooconfigure integrace Azure AD s Learning stanici pro správu vzdělávacího procesu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="02c08-112">tooconfigure Azure AD integration with Learning Seat LMS, you need hello following items:</span></span>

- <span data-ttu-id="02c08-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="02c08-113">An Azure AD subscription</span></span>
- <span data-ttu-id="02c08-114">Stanici Learning vzdělávacího procesu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="02c08-114">A Learning Seat LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="02c08-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="02c08-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="02c08-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="02c08-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="02c08-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="02c08-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="02c08-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="02c08-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="02c08-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="02c08-119">Scenario description</span></span>
<span data-ttu-id="02c08-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02c08-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="02c08-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="02c08-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="02c08-122">Přidání Learning stanici pro správu vzdělávacího procesu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="02c08-122">Adding Learning Seat LMS from hello gallery</span></span>
2. <span data-ttu-id="02c08-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="02c08-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-seat-lms-from-hello-gallery"></a><span data-ttu-id="02c08-124">Přidání Learning stanici pro správu vzdělávacího procesu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="02c08-124">Adding Learning Seat LMS from hello gallery</span></span>
<span data-ttu-id="02c08-125">tooconfigure hello integrace Learning stanici pro správu vzdělávacího procesu do Azure AD, je nutné tooadd Learning stanici pro správu vzdělávacího procesu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="02c08-125">tooconfigure hello integration of Learning Seat LMS into Azure AD, you need tooadd Learning Seat LMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="02c08-126">**tooadd vzdělávacího procesu stanici Learning z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02c08-126">**tooadd Learning Seat LMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c08-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="02c08-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="02c08-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="02c08-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="02c08-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="02c08-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="02c08-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02c08-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="02c08-134">Hello vyhledávacího pole zadejte **Learning stanici pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="02c08-134">In hello search box, type **Learning Seat LMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_search.png)

5. <span data-ttu-id="02c08-136">Na panelu výsledků hello vyberte **Learning stanici pro správu vzdělávacího procesu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="02c08-136">In hello results panel, select **Learning Seat LMS**, and then click **Add** button tooadd hello application.</span></span>


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="02c08-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="02c08-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="02c08-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="02c08-138">In this section, you configure and test Azure AD single sign-on with Learning Seat LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="02c08-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Learning stanici pro správu vzdělávacího procesu je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="02c08-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learning Seat LMS is tooa user in Azure AD.</span></span> <span data-ttu-id="02c08-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Learning stanici pro správu vzdělávacího procesu musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="02c08-140">In other words, a link relationship between an Azure AD user and hello related user in Learning Seat LMS needs toobe established.</span></span>

<span data-ttu-id="02c08-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Learning stanici pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="02c08-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Learning Seat LMS.</span></span>

<span data-ttu-id="02c08-142">tooconfigure a testu Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="02c08-142">tooconfigure and test Azure AD single sign-on with Learning Seat LMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="02c08-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="02c08-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="02c08-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="02c08-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="02c08-145">**[Vytvoření zkušebního uživatele Learning stanici pro správu vzdělávacího procesu](#creating-a-learnconnect-test-user)**  -toohave protějšek Britta Simon v vzdělávacího procesu Learning stanici, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="02c08-145">**[Creating a Learning Seat LMS test user](#creating-a-learnconnect-test-user)** - toohave a counterpart of Britta Simon in Learning Seat LMS that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="02c08-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02c08-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="02c08-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="02c08-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="02c08-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="02c08-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="02c08-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci pro správu vzdělávacího procesu Learning stanici.</span><span class="sxs-lookup"><span data-stu-id="02c08-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learning Seat LMS application.</span></span>

<span data-ttu-id="02c08-150">**tooconfigure Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02c08-150">**tooconfigure Azure AD single sign-on with Learning Seat LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c08-151">V portálu Azure, na hello hello **Learning stanici pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="02c08-151">In hello Azure portal, on hello **Learning Seat LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="02c08-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02c08-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_samlbase.png)

3. <span data-ttu-id="02c08-155">Na hello **Learning stanici pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="02c08-155">On hello **Learning Seat LMS Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url.png)

    <span data-ttu-id="02c08-157">a.</span><span class="sxs-lookup"><span data-stu-id="02c08-157">a.</span></span> <span data-ttu-id="02c08-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="02c08-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com`</span></span>

    <span data-ttu-id="02c08-159">b.</span><span class="sxs-lookup"><span data-stu-id="02c08-159">b.</span></span> <span data-ttu-id="02c08-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span><span class="sxs-lookup"><span data-stu-id="02c08-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span></span>

4. <span data-ttu-id="02c08-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="02c08-161">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url2.png)

    <span data-ttu-id="02c08-163">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="02c08-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.learningseatlms.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="02c08-164">Tyto hodnoty nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02c08-164">These values are not hello real values.</span></span> <span data-ttu-id="02c08-165">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="02c08-165">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="02c08-166">Obraťte se na [tým podpory Learning stanici](http://help.learningseatlms.com/help) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02c08-166">Contact [Learning Seat support team](http://help.learningseatlms.com/help) tooget these values.</span></span> 

5. <span data-ttu-id="02c08-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="02c08-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_certificate.png) 

6. <span data-ttu-id="02c08-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02c08-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="02c08-171">tooconfigure jednotného přihlašování na **Learning stanici pro správu vzdělávacího procesu** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Learning stanici](http://help.learningseatlms.com/help).</span><span class="sxs-lookup"><span data-stu-id="02c08-171">tooconfigure single sign-on on **Learning Seat LMS** side, you need toosend hello downloaded **Metadata XML** too[Learning Seat support team](http://help.learningseatlms.com/help).</span></span>

> [!TIP]
> <span data-ttu-id="02c08-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="02c08-172">You can now read a concise version of these instructions inside hello [Azure  portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="02c08-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="02c08-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="02c08-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="02c08-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="02c08-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="02c08-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="02c08-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="02c08-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="02c08-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02c08-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c08-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="02c08-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="02c08-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="02c08-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="02c08-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="02c08-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="02c08-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="02c08-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="02c08-187">a.</span><span class="sxs-lookup"><span data-stu-id="02c08-187">a.</span></span> <span data-ttu-id="02c08-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="02c08-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="02c08-189">b.</span><span class="sxs-lookup"><span data-stu-id="02c08-189">b.</span></span> <span data-ttu-id="02c08-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="02c08-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="02c08-191">c.</span><span class="sxs-lookup"><span data-stu-id="02c08-191">c.</span></span> <span data-ttu-id="02c08-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="02c08-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="02c08-193">d.</span><span class="sxs-lookup"><span data-stu-id="02c08-193">d.</span></span> <span data-ttu-id="02c08-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="02c08-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-seat-lms-test-user"></a><span data-ttu-id="02c08-195">Vytvoření zkušebního uživatele Learning stanici pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="02c08-195">Creating a Learning Seat LMS test user</span></span>

<span data-ttu-id="02c08-196">V této části vytvoříte uživatele volal Britta Simon v Learning stanici pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="02c08-196">In this section, you create a user called Britta Simon in Learning Seat LMS.</span></span> <span data-ttu-id="02c08-197">Obraťte se na [tým podpory Learning stanici](http://help.learningseatlms.com/help) všechny hello uživatelské informace tooadd hello uživatelům v hello Learning stanici pro správu vzdělávacího procesu aplikace.</span><span class="sxs-lookup"><span data-stu-id="02c08-197">Contact [Learning Seat support team](http://help.learningseatlms.com/help) with all hello user information tooadd hello users in hello Learning Seat LMS application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="02c08-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="02c08-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="02c08-199">V této části povolíte tak, že udělíte přístup tooLearning stanici pro správu vzdělávacího procesu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02c08-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearning Seat LMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="02c08-201">**tooassign tooLearning Britta Simon stanici pro správu vzdělávacího procesu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02c08-201">**tooassign Britta Simon tooLearning Seat LMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="02c08-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="02c08-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="02c08-204">V seznamu aplikace hello vyberte **Learning stanici pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="02c08-204">In hello applications list, select **Learning Seat LMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_app.png) 

3. <span data-ttu-id="02c08-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="02c08-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="02c08-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02c08-208">Click **Add** button.</span></span> <span data-ttu-id="02c08-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02c08-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="02c08-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="02c08-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="02c08-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02c08-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="02c08-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02c08-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="02c08-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="02c08-214">Testing single sign-on</span></span>

<span data-ttu-id="02c08-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="02c08-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> 

<span data-ttu-id="02c08-216">Klikněte na tlačítko hello Learning stanici pro správu vzdělávacího procesu dlaždici v hello přístupového panelu, budou automaticky přihlášeného tooyour Learning stanici pro správu vzdělávacího procesu aplikace.</span><span class="sxs-lookup"><span data-stu-id="02c08-216">Click hello Learning Seat LMS tile in hello Access Panel, you will be automatically signed-on tooyour Learning Seat LMS application.</span></span> <span data-ttu-id="02c08-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="02c08-217">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02c08-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="02c08-218">Additional resources</span></span>

* [<span data-ttu-id="02c08-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="02c08-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="02c08-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="02c08-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_203.png

