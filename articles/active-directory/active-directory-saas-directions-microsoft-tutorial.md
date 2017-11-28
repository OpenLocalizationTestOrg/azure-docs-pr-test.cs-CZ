---
title: "Kurz: Azure Active Directory integrace s pokyny, společnost Microsoft | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a pokynů společnost Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 59a8b856fd2dc75a37e9bb8f46ced066bcd0fd56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="19dc7-103">Kurz: Azure Active Directory integrace s pokyny, společnost Microsoft</span><span class="sxs-lookup"><span data-stu-id="19dc7-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="19dc7-104">V tomto kurzu zjistíte, jak toointegrate pokynů na Microsoft s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="19dc7-104">In this tutorial, you learn how toointegrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="19dc7-105">Integrace s Azure AD pokynů společnost Microsoft poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="19dc7-105">Integrating Directions on Microsoft with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="19dc7-106">Můžete řídit ve službě Azure AD, který má přístup tooDirections společnost Microsoft</span><span class="sxs-lookup"><span data-stu-id="19dc7-106">You can control in Azure AD who has access tooDirections on Microsoft</span></span>
- <span data-ttu-id="19dc7-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDirections společnost Microsoft (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="19dc7-107">You can enable your users tooautomatically get signed-on tooDirections on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="19dc7-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="19dc7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="19dc7-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="19dc7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19dc7-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="19dc7-110">Prerequisites</span></span>

<span data-ttu-id="19dc7-111">tooconfigure integrace Azure AD s pokyny, společnost Microsoft, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="19dc7-111">tooconfigure Azure AD integration with Directions on Microsoft, you need hello following items:</span></span>

- <span data-ttu-id="19dc7-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="19dc7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="19dc7-113">Předplatné povolené pokynů na Microsoft Jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="19dc7-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="19dc7-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="19dc7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="19dc7-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="19dc7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="19dc7-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="19dc7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="19dc7-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="19dc7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="19dc7-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="19dc7-118">Scenario description</span></span>
<span data-ttu-id="19dc7-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="19dc7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="19dc7-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="19dc7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="19dc7-121">Přidání pokynů na Microsoft z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="19dc7-121">Adding Directions on Microsoft from hello gallery</span></span>
2. <span data-ttu-id="19dc7-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="19dc7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-hello-gallery"></a><span data-ttu-id="19dc7-123">Přidání pokynů na Microsoft z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="19dc7-123">Adding Directions on Microsoft from hello gallery</span></span>
<span data-ttu-id="19dc7-124">integrace hello tooconfigure pokynů na Microsoft do služby Azure AD, je nutné tooadd pokynů na Microsoft hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="19dc7-124">tooconfigure hello integration of Directions on Microsoft into Azure AD, you need tooadd Directions on Microsoft from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="19dc7-125">**tooadd pokynů na Microsoft z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="19dc7-125">**tooadd Directions on Microsoft from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="19dc7-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="19dc7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="19dc7-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="19dc7-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="19dc7-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19dc7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="19dc7-133">Hello vyhledávacího pole zadejte **pokynů na Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-133">In hello search box, type **Directions on Microsoft**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="19dc7-135">Na panelu výsledků hello vyberte **pokynů na Microsoft**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="19dc7-135">In hello results panel, select **Directions on Microsoft**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="19dc7-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="19dc7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="19dc7-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pokyny týkající se společnosti Microsoft podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="19dc7-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="19dc7-139">Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v směrech společnost Microsoft je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="19dc7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Directions on Microsoft is tooa user in Azure AD.</span></span> <span data-ttu-id="19dc7-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v směrech na Microsoft musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="19dc7-140">In other words, a link relationship between an Azure AD user and hello related user in Directions on Microsoft needs toobe established.</span></span>

<span data-ttu-id="19dc7-141">V pokynů na Microsoft přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="19dc7-141">In Directions on Microsoft, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="19dc7-142">tooconfigure a testu Azure AD jednotné přihlašování s pokyny, společnost Microsoft, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="19dc7-142">tooconfigure and test Azure AD single sign-on with Directions on Microsoft, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="19dc7-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="19dc7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="19dc7-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="19dc7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="19dc7-145">**[Vytváření pokynů na Microsoft testovacího uživatele](#creating-a-directions-on-microsoft-test-user)**  -toohave protějšek Britta Simon v směrech na společnosti Microsoft, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="19dc7-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - toohave a counterpart of Britta Simon in Directions on Microsoft that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="19dc7-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="19dc7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="19dc7-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="19dc7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="19dc7-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="19dc7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="19dc7-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v vaší směrech na aplikaci Microsoft.</span><span class="sxs-lookup"><span data-stu-id="19dc7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="19dc7-150">**tooconfigure Azure AD jednotné přihlašování pokyny týkající se společnosti Microsoft, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="19dc7-150">**tooconfigure Azure AD single sign-on with Directions on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="19dc7-151">V portálu Azure, na hello hello **pokynů na Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-151">In hello Azure portal, on hello **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="19dc7-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="19dc7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="19dc7-155">Na hello **pokynů na Microsoft Domain a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="19dc7-155">On hello **Directions on Microsoft Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="19dc7-157">a.</span><span class="sxs-lookup"><span data-stu-id="19dc7-157">a.</span></span> <span data-ttu-id="19dc7-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="19dc7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="19dc7-159">b.</span><span class="sxs-lookup"><span data-stu-id="19dc7-159">b.</span></span> <span data-ttu-id="19dc7-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="19dc7-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="19dc7-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="19dc7-161">These values are not real.</span></span> <span data-ttu-id="19dc7-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="19dc7-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="19dc7-163">Obraťte se na [tým podpory pokynů na Microsoft Client](mailto:service@DirectionsOnMicrosoft.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="19dc7-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="19dc7-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="19dc7-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="19dc7-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19dc7-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="19dc7-168">tooconfigure jednotného přihlašování na **pokynů na Microsoft** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory pokynů na Microsoft](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="19dc7-168">tooconfigure single sign-on on **Directions on Microsoft** side, you need toosend hello downloaded **Metadata XML** too[Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="19dc7-169">tooenable hello pokynů na Microsoft podporují team toolocate členstvím na vašem federovaného webu, zahrnují informace o vaší společnosti v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="19dc7-169">tooenable hello Directions on Microsoft support team toolocate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="19dc7-170">Jednotné přihlašování pro pokynů na Microsoft musí toobe ve hello povolené [tým podpory pokynů na Microsoft Client](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="19dc7-170">Single sign-on for Directions on Microsoft needs toobe enabled by hello [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="19dc7-171">Zobrazí se oznámení a pokud jednotné přihlašování je povolena.</span><span class="sxs-lookup"><span data-stu-id="19dc7-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="19dc7-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="19dc7-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="19dc7-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="19dc7-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="19dc7-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="19dc7-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="19dc7-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="19dc7-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="19dc7-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="19dc7-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="19dc7-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="19dc7-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="19dc7-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="19dc7-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="19dc7-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="19dc7-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="19dc7-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="19dc7-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="19dc7-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="19dc7-187">a.</span><span class="sxs-lookup"><span data-stu-id="19dc7-187">a.</span></span> <span data-ttu-id="19dc7-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="19dc7-189">b.</span><span class="sxs-lookup"><span data-stu-id="19dc7-189">b.</span></span> <span data-ttu-id="19dc7-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="19dc7-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="19dc7-191">c.</span><span class="sxs-lookup"><span data-stu-id="19dc7-191">c.</span></span> <span data-ttu-id="19dc7-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="19dc7-193">d.</span><span class="sxs-lookup"><span data-stu-id="19dc7-193">d.</span></span> <span data-ttu-id="19dc7-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="19dc7-195">Vytváření pokynů na Microsoft testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="19dc7-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="19dc7-196">Neexistuje žádná položka akce pro jste tooconfigure zřizování uživatelů tooDirections společnost Microsoft.</span><span class="sxs-lookup"><span data-stu-id="19dc7-196">There is no action item for you tooconfigure user provisioning tooDirections on Microsoft.</span></span>  

<span data-ttu-id="19dc7-197">Když se uživatel s přiřazenou pokusí toolog v tooDirections na Microsoft použije hello přístupového panelu, pokynů na Microsoft ověří, zda existuje hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="19dc7-197">When an assigned user tries toolog in tooDirections on Microsoft using hello access panel, Directions on Microsoft checks whether hello user exists.</span></span> <span data-ttu-id="19dc7-198">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří podle pokynů na Microsoft.</span><span class="sxs-lookup"><span data-stu-id="19dc7-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="19dc7-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="19dc7-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="19dc7-200">V této části povolíte tak, že udělíte přístup tooDirections na Microsoft toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="19dc7-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirections on Microsoft.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="19dc7-202">**tooassign tooDirections Britta Simon společnost Microsoft, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="19dc7-202">**tooassign Britta Simon tooDirections on Microsoft, perform hello following steps:**</span></span>

1. <span data-ttu-id="19dc7-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="19dc7-205">V seznamu aplikace hello vyberte **pokynů na Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-205">In hello applications list, select **Directions on Microsoft**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="19dc7-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="19dc7-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="19dc7-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="19dc7-209">Click **Add** button.</span></span> <span data-ttu-id="19dc7-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="19dc7-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="19dc7-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="19dc7-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="19dc7-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="19dc7-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="19dc7-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="19dc7-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="19dc7-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="19dc7-215">Testing single sign-on</span></span>

<span data-ttu-id="19dc7-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="19dc7-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="19dc7-217">Po kliknutí na tlačítko hello pokynů na dlaždici Microsoft v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour pokynů na aplikaci Microsoft.</span><span class="sxs-lookup"><span data-stu-id="19dc7-217">When you click hello Directions on Microsoft tile in hello Access Panel, you should get automatically signed-on tooyour Directions on Microsoft application.</span></span>

<span data-ttu-id="19dc7-218">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="19dc7-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="19dc7-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="19dc7-219">Additional resources</span></span>

* [<span data-ttu-id="19dc7-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="19dc7-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="19dc7-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="19dc7-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

