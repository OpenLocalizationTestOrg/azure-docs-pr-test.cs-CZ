---
title: 'Kurz: Azure Active Directory integrace s MobileXpense | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a MobileXpense."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e649fc4e-3e15-4948-b977-00bfe9f7db13
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: jeedes
ms.openlocfilehash: b9d109f9d4244f8a7eb8b49b0d980cd3df0fc59d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mobilexpense"></a><span data-ttu-id="0eb41-103">Kurz: Azure Active Directory integrace s MobileXpense</span><span class="sxs-lookup"><span data-stu-id="0eb41-103">Tutorial: Azure Active Directory integration with MobileXpense</span></span>

<span data-ttu-id="0eb41-104">V tomto kurzu zjistíte, jak toointegrate MobileXpense s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0eb41-104">In this tutorial, you learn how toointegrate MobileXpense with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0eb41-105">Integrace MobileXpense s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0eb41-105">Integrating MobileXpense with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0eb41-106">Můžete řídit ve službě Azure AD, který má přístup tooMobileXpense</span><span class="sxs-lookup"><span data-stu-id="0eb41-106">You can control in Azure AD who has access tooMobileXpense</span></span>
- <span data-ttu-id="0eb41-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooMobileXpense (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0eb41-107">You can enable your users tooautomatically get signed-on tooMobileXpense (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0eb41-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0eb41-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0eb41-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0eb41-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0eb41-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0eb41-110">Prerequisites</span></span>

<span data-ttu-id="0eb41-111">Integrace služby Azure AD s MobileXpense tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="0eb41-111">tooconfigure Azure AD integration with MobileXpense, you need hello following items:</span></span>

- <span data-ttu-id="0eb41-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0eb41-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0eb41-113">MobileXpense jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0eb41-113">A MobileXpense single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0eb41-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0eb41-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0eb41-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0eb41-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0eb41-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0eb41-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0eb41-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0eb41-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0eb41-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0eb41-118">Scenario description</span></span>
<span data-ttu-id="0eb41-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0eb41-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0eb41-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0eb41-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0eb41-121">Přidání MobileXpense z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0eb41-121">Adding MobileXpense from hello gallery</span></span>
2. <span data-ttu-id="0eb41-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0eb41-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mobilexpense-from-hello-gallery"></a><span data-ttu-id="0eb41-123">Přidání MobileXpense z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="0eb41-123">Adding MobileXpense from hello gallery</span></span>
<span data-ttu-id="0eb41-124">tooconfigure hello integrace MobileXpense do Azure AD, je nutné tooadd MobileXpense hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="0eb41-124">tooconfigure hello integration of MobileXpense into Azure AD, you need tooadd MobileXpense from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0eb41-125">**tooadd MobileXpense z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0eb41-125">**tooadd MobileXpense from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0eb41-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0eb41-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0eb41-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0eb41-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0eb41-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0eb41-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0eb41-133">Hello vyhledávacího pole zadejte **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-133">In hello search box, type **MobileXpense**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_search.png)

5. <span data-ttu-id="0eb41-135">Na panelu výsledků hello vyberte **MobileXpense**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eb41-135">In hello results panel, select **MobileXpense**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0eb41-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0eb41-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0eb41-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s MobileXpense podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0eb41-138">In this section, you configure and test Azure AD single sign-on with MobileXpense based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0eb41-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v MobileXpense je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0eb41-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MobileXpense is tooa user in Azure AD.</span></span> <span data-ttu-id="0eb41-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v MobileXpense musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="0eb41-140">In other words, a link relationship between an Azure AD user and hello related user in MobileXpense needs toobe established.</span></span>

<span data-ttu-id="0eb41-141">V MobileXpense, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="0eb41-141">In MobileXpense, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0eb41-142">tooconfigure a testu Azure AD jednotné přihlašování s MobileXpense, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="0eb41-142">tooconfigure and test Azure AD single sign-on with MobileXpense, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0eb41-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="0eb41-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0eb41-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0eb41-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0eb41-145">**[Vytvoření zkušebního uživatele MobileXpense](#creating-a-mobilexpense-test-user)**  -toohave protějšek Britta Simon v MobileXpense, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="0eb41-145">**[Creating a MobileXpense test user](#creating-a-mobilexpense-test-user)** - toohave a counterpart of Britta Simon in MobileXpense that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0eb41-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0eb41-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0eb41-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="0eb41-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0eb41-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0eb41-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0eb41-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci MobileXpense.</span><span class="sxs-lookup"><span data-stu-id="0eb41-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MobileXpense application.</span></span>

<span data-ttu-id="0eb41-150">**tooconfigure Azure AD jednotné přihlašování s MobileXpense, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0eb41-150">**tooconfigure Azure AD single sign-on with MobileXpense, perform hello following steps:**</span></span>

1. <span data-ttu-id="0eb41-151">V portálu Azure, na hello hello **MobileXpense** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-151">In hello Azure portal, on hello **MobileXpense** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0eb41-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0eb41-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_samlbase.png)

3. <span data-ttu-id="0eb41-155">Na hello **MobileXpense domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0eb41-155">On hello **MobileXpense Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url11.png)

    <span data-ttu-id="0eb41-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span><span class="sxs-lookup"><span data-stu-id="0eb41-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<sub domain>.mobilexpense.com/SSO/SAML20/SAML/AssertionConsumerService.aspx`</span></span>

4. <span data-ttu-id="0eb41-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="0eb41-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_url22.png)

<span data-ttu-id="0eb41-160">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzor::`https://<sub domain>.mobilexpense.com/<customername>`</span><span class="sxs-lookup"><span data-stu-id="0eb41-160">In hello **Sign-on URL** textbox, type a URL using hello following pattern:: `https://<sub domain>.mobilexpense.com/<customername>`</span></span>

> [!NOTE] 
> <span data-ttu-id="0eb41-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="0eb41-161">These values are not real.</span></span> <span data-ttu-id="0eb41-162">Tyto hodnoty aktualizujte hello skutečná adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="0eb41-162">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="0eb41-163">Obraťte se na [tým podpory MobileXpense klienta](http://www.mobilexpense.net/contact) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0eb41-163">Contact [MobileXpense Client support team](http://www.mobilexpense.net/contact) tooget these values.</span></span> 

5. <span data-ttu-id="0eb41-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="0eb41-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_certificate.png) 

6. <span data-ttu-id="0eb41-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0eb41-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0eb41-168">tooconfigure jednotného přihlašování na **MobileXpense** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory MobileXpense](http://www.mobilexpense.net/contact).</span><span class="sxs-lookup"><span data-stu-id="0eb41-168">tooconfigure single sign-on on **MobileXpense** side, you need toosend hello downloaded **Metadata XML** too[MobileXpense support team](http://www.mobilexpense.net/contact).</span></span>

> [!TIP]
> <span data-ttu-id="0eb41-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="0eb41-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0eb41-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="0eb41-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0eb41-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0eb41-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0eb41-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0eb41-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="0eb41-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="0eb41-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0eb41-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0eb41-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0eb41-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0eb41-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0eb41-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0eb41-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="0eb41-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0eb41-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0eb41-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mobilexpense-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0eb41-184">a.</span><span class="sxs-lookup"><span data-stu-id="0eb41-184">a.</span></span> <span data-ttu-id="0eb41-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0eb41-186">b.</span><span class="sxs-lookup"><span data-stu-id="0eb41-186">b.</span></span> <span data-ttu-id="0eb41-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0eb41-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0eb41-188">c.</span><span class="sxs-lookup"><span data-stu-id="0eb41-188">c.</span></span> <span data-ttu-id="0eb41-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0eb41-190">d.</span><span class="sxs-lookup"><span data-stu-id="0eb41-190">d.</span></span> <span data-ttu-id="0eb41-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-191">Click **Create**.</span></span>
 
### <a name="creating-a-mobilexpense-test-user"></a><span data-ttu-id="0eb41-192">Vytvoření zkušebního uživatele MobileXpense</span><span class="sxs-lookup"><span data-stu-id="0eb41-192">Creating a MobileXpense test user</span></span>

<span data-ttu-id="0eb41-193">V této části vytvoříte volal Britta Simon v MobileXpense uživatele.</span><span class="sxs-lookup"><span data-stu-id="0eb41-193">In this section, you create a user called Britta Simon in MobileXpense.</span></span> <span data-ttu-id="0eb41-194">Práce s [tým podpory MobileXpense](http://www.mobilexpense.net/contact) pro přidání uživatelů hello hello MobileXpense platformy.</span><span class="sxs-lookup"><span data-stu-id="0eb41-194">work with [MobileXpense support team](http://www.mobilexpense.net/contact) to add hello users in hello MobileXpense platform.</span></span> <span data-ttu-id="0eb41-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0eb41-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0eb41-196">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="0eb41-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0eb41-197">V této části povolíte tak, že udělíte přístup tooMobileXpense toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0eb41-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMobileXpense.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0eb41-199">**tooassign Britta Simon tooMobileXpense, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="0eb41-199">**tooassign Britta Simon tooMobileXpense, perform hello following steps:**</span></span>

1. <span data-ttu-id="0eb41-200">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0eb41-202">V seznamu aplikace hello vyberte **MobileXpense**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-202">In hello applications list, select **MobileXpense**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mobilexpense-tutorial/tutorial_mobilexpense_app.png) 

3. <span data-ttu-id="0eb41-204">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0eb41-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0eb41-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0eb41-206">Click **Add** button.</span></span> <span data-ttu-id="0eb41-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0eb41-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0eb41-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="0eb41-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0eb41-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0eb41-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0eb41-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0eb41-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0eb41-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0eb41-212">Testing single sign-on</span></span>

<span data-ttu-id="0eb41-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0eb41-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0eb41-214">Když kliknete na dlaždici MobileXpense hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour MobileXpense aplikace.</span><span class="sxs-lookup"><span data-stu-id="0eb41-214">When you click hello MobileXpense tile in hello Access Panel, you should get automatically signed-on tooyour MobileXpense application.</span></span>
<span data-ttu-id="0eb41-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="0eb41-215">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0eb41-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0eb41-216">Additional resources</span></span>

* [<span data-ttu-id="0eb41-217">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0eb41-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0eb41-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0eb41-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mobilexpense-tutorial/tutorial_general_203.png

