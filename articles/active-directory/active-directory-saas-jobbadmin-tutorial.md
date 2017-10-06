---
title: 'Kurz: Azure Active Directory integrace s Jobbadmin | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jobbadmin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c5208b0d-66a3-49ed-9aad-70d21f54aee0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0796abd2934c0f94648b2c11e7fdf69304f835c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobbadmin"></a><span data-ttu-id="50bda-103">Kurz: Azure Active Directory integrace s Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="50bda-103">Tutorial: Azure Active Directory integration with Jobbadmin</span></span>

<span data-ttu-id="50bda-104">V tomto kurzu zjistíte, jak toointegrate Jobbadmin s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="50bda-104">In this tutorial, you learn how toointegrate Jobbadmin with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="50bda-105">Integrace Jobbadmin s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="50bda-105">Integrating Jobbadmin with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="50bda-106">Můžete řídit ve službě Azure AD, který má přístup tooJobbadmin</span><span class="sxs-lookup"><span data-stu-id="50bda-106">You can control in Azure AD who has access tooJobbadmin</span></span>
- <span data-ttu-id="50bda-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooJobbadmin (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="50bda-107">You can enable your users tooautomatically get signed-on tooJobbadmin (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="50bda-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="50bda-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="50bda-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50bda-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50bda-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="50bda-110">Prerequisites</span></span>

<span data-ttu-id="50bda-111">Integrace služby Azure AD s Jobbadmin tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="50bda-111">tooconfigure Azure AD integration with Jobbadmin, you need hello following items:</span></span>

- <span data-ttu-id="50bda-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="50bda-112">An Azure AD subscription</span></span>
- <span data-ttu-id="50bda-113">Jobbadmin jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="50bda-113">A Jobbadmin single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="50bda-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="50bda-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="50bda-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="50bda-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="50bda-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="50bda-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="50bda-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="50bda-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="50bda-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="50bda-118">Scenario description</span></span>
<span data-ttu-id="50bda-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="50bda-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="50bda-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="50bda-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="50bda-121">Přidání Jobbadmin z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="50bda-121">Adding Jobbadmin from hello gallery</span></span>
2. <span data-ttu-id="50bda-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="50bda-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobbadmin-from-hello-gallery"></a><span data-ttu-id="50bda-123">Přidání Jobbadmin z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="50bda-123">Adding Jobbadmin from hello gallery</span></span>
<span data-ttu-id="50bda-124">tooconfigure hello integrace Jobbadmin do Azure AD, je nutné tooadd Jobbadmin hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="50bda-124">tooconfigure hello integration of Jobbadmin into Azure AD, you need tooadd Jobbadmin from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="50bda-125">**tooadd Jobbadmin z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50bda-125">**tooadd Jobbadmin from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="50bda-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="50bda-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="50bda-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="50bda-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="50bda-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="50bda-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="50bda-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="50bda-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="50bda-133">Hello vyhledávacího pole zadejte **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="50bda-133">In hello search box, type **Jobbadmin**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_search.png)

5. <span data-ttu-id="50bda-135">Na panelu výsledků hello vyberte **Jobbadmin**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="50bda-135">In hello results panel, select **Jobbadmin**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="50bda-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="50bda-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="50bda-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobbadmin podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="50bda-138">In this section, you configure and test Azure AD single sign-on with Jobbadmin based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="50bda-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Jobbadmin je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50bda-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jobbadmin is tooa user in Azure AD.</span></span> <span data-ttu-id="50bda-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Jobbadmin musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="50bda-140">In other words, a link relationship between an Azure AD user and hello related user in Jobbadmin needs toobe established.</span></span>

<span data-ttu-id="50bda-141">V Jobbadmin, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="50bda-141">In Jobbadmin, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="50bda-142">tooconfigure a testu Azure AD jednotné přihlašování s Jobbadmin, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="50bda-142">tooconfigure and test Azure AD single sign-on with Jobbadmin, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="50bda-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="50bda-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="50bda-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="50bda-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="50bda-145">**[Vytvoření zkušebního uživatele Jobbadmin](#creating-a-jobbadmin-test-user)**  -toohave protějšek Britta Simon v Jobbadmin, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="50bda-145">**[Creating a Jobbadmin test user](#creating-a-jobbadmin-test-user)** - toohave a counterpart of Britta Simon in Jobbadmin that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="50bda-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="50bda-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="50bda-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="50bda-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="50bda-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="50bda-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="50bda-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Jobbadmin.</span><span class="sxs-lookup"><span data-stu-id="50bda-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jobbadmin application.</span></span>

<span data-ttu-id="50bda-150">**tooconfigure Azure AD jednotné přihlašování s Jobbadmin, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50bda-150">**tooconfigure Azure AD single sign-on with Jobbadmin, perform hello following steps:**</span></span>

1. <span data-ttu-id="50bda-151">V portálu Azure, na hello hello **Jobbadmin** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="50bda-151">In hello Azure portal, on hello **Jobbadmin** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="50bda-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="50bda-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_samlbase.png)

3. <span data-ttu-id="50bda-155">Na hello **Jobbadmin domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="50bda-155">On hello **Jobbadmin Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_url.png)

    <span data-ttu-id="50bda-157">a.</span><span class="sxs-lookup"><span data-stu-id="50bda-157">a.</span></span> <span data-ttu-id="50bda-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="50bda-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    <span data-ttu-id="50bda-159">b.</span><span class="sxs-lookup"><span data-stu-id="50bda-159">b.</span></span> <span data-ttu-id="50bda-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.jobnorge.no`</span><span class="sxs-lookup"><span data-stu-id="50bda-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.jobnorge.no`</span></span>

    <span data-ttu-id="50bda-161">c.</span><span class="sxs-lookup"><span data-stu-id="50bda-161">c.</span></span> <span data-ttu-id="50bda-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="50bda-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="50bda-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="50bda-163">These values are not real.</span></span> <span data-ttu-id="50bda-164">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="50bda-164">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="50bda-165">Obraťte se na [tým podpory Jobbadmin klienta](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="50bda-165">Contact [Jobbadmin Client support team](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget these values.</span></span> 
 


4. <span data-ttu-id="50bda-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="50bda-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_certificate.png) 

5. <span data-ttu-id="50bda-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="50bda-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="50bda-170">tooconfigure jednotného přihlašování na **Jobbadmin** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Jobbadmin](https://www.jobbnorge.no/om-oss/kontakt-oss).</span><span class="sxs-lookup"><span data-stu-id="50bda-170">tooconfigure single sign-on on **Jobbadmin** side, you need toosend hello downloaded **Metadata XML** too[Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss).</span></span> <span data-ttu-id="50bda-171">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="50bda-171">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="50bda-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="50bda-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="50bda-173">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="50bda-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="50bda-174">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="50bda-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="50bda-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="50bda-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="50bda-176">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="50bda-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="50bda-178">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50bda-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="50bda-179">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="50bda-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="50bda-181">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="50bda-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="50bda-183">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="50bda-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="50bda-185">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="50bda-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="50bda-187">a.</span><span class="sxs-lookup"><span data-stu-id="50bda-187">a.</span></span> <span data-ttu-id="50bda-188">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="50bda-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="50bda-189">b.</span><span class="sxs-lookup"><span data-stu-id="50bda-189">b.</span></span> <span data-ttu-id="50bda-190">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="50bda-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="50bda-191">c.</span><span class="sxs-lookup"><span data-stu-id="50bda-191">c.</span></span> <span data-ttu-id="50bda-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="50bda-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="50bda-193">d.</span><span class="sxs-lookup"><span data-stu-id="50bda-193">d.</span></span> <span data-ttu-id="50bda-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="50bda-194">Click **Create**.</span></span>
 
### <a name="creating-a-jobbadmin-test-user"></a><span data-ttu-id="50bda-195">Vytvoření zkušebního uživatele Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="50bda-195">Creating a Jobbadmin test user</span></span>

<span data-ttu-id="50bda-196">Uživatelé toolog tooenable Azure AD v tooJobbadmin, se musí být zřízená do Jobbadmin.</span><span class="sxs-lookup"><span data-stu-id="50bda-196">tooenable Azure AD users toolog in tooJobbadmin, they must be provisioned into Jobbadmin.</span></span>
 
<span data-ttu-id="50bda-197">Obraťte se na [tým podpory Jobbadmin](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget hello uživatelé přidat na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="50bda-197">Please contact [Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss) tooget hello users added on their side.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="50bda-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="50bda-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="50bda-199">V této části povolíte tak, že udělíte přístup tooJobbadmin toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="50bda-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobbadmin.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="50bda-201">**tooassign Britta Simon tooJobbadmin, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="50bda-201">**tooassign Britta Simon tooJobbadmin, perform hello following steps:**</span></span>

1. <span data-ttu-id="50bda-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="50bda-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="50bda-204">V seznamu aplikace hello vyberte **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="50bda-204">In hello applications list, select **Jobbadmin**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_app.png) 

3. <span data-ttu-id="50bda-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="50bda-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="50bda-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="50bda-208">Click **Add** button.</span></span> <span data-ttu-id="50bda-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="50bda-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="50bda-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="50bda-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="50bda-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="50bda-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="50bda-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="50bda-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="50bda-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="50bda-214">Testing single sign-on</span></span>

<span data-ttu-id="50bda-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="50bda-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="50bda-216">Když kliknete na dlaždici Jobbadmin hello v hello přístupového panelu, měli byste obdržet přihlašovací stránku Jobbadmin aplikace.</span><span class="sxs-lookup"><span data-stu-id="50bda-216">When you click hello Jobbadmin tile in hello Access Panel, you should get login page of Jobbadmin application.</span></span>
<span data-ttu-id="50bda-217">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50bda-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="50bda-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="50bda-218">Additional resources</span></span>

* [<span data-ttu-id="50bda-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50bda-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="50bda-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="50bda-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_203.png

