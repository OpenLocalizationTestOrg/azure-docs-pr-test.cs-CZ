---
title: "Kurz: Azure Active Directory integrace s e-učení Yardi | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a e-Yardi učení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7ea58b54-ec5b-4576-8586-814b11d0f4fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 47c95fe024e76a67aa5c5b3ee6f81cbafc50ff07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yardi-elearning"></a><span data-ttu-id="8620c-103">Kurz: Azure Active Directory integrace s e-učení Yardi</span><span class="sxs-lookup"><span data-stu-id="8620c-103">Tutorial: Azure Active Directory integration with Yardi eLearning</span></span>

<span data-ttu-id="8620c-104">V tomto kurzu zjistíte, jak toointegrate Yardi e-učení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8620c-104">In this tutorial, you learn how toointegrate Yardi eLearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8620c-105">Integrace Yardi e-učení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8620c-105">Integrating Yardi eLearning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8620c-106">Můžete řídit ve službě Azure AD, který má přístup tooYardi e-učení</span><span class="sxs-lookup"><span data-stu-id="8620c-106">You can control in Azure AD who has access tooYardi eLearning</span></span>
- <span data-ttu-id="8620c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooYardi e-učení (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8620c-107">You can enable your users tooautomatically get signed-on tooYardi eLearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8620c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8620c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8620c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8620c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8620c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8620c-110">Prerequisites</span></span>

<span data-ttu-id="8620c-111">Integrace služby Azure AD s e-učení Yardi tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8620c-111">tooconfigure Azure AD integration with Yardi eLearning, you need hello following items:</span></span>

- <span data-ttu-id="8620c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8620c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8620c-113">Yardi e-učení jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8620c-113">A Yardi eLearning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8620c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8620c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8620c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8620c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8620c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8620c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8620c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8620c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8620c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8620c-118">Scenario description</span></span>
<span data-ttu-id="8620c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8620c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8620c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8620c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8620c-121">Přidání e-učení Yardi z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8620c-121">Adding Yardi eLearning from hello gallery</span></span>
2. <span data-ttu-id="8620c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8620c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yardi-elearning-from-hello-gallery"></a><span data-ttu-id="8620c-123">Přidání e-učení Yardi z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8620c-123">Adding Yardi eLearning from hello gallery</span></span>
<span data-ttu-id="8620c-124">tooconfigure hello integraci e-učení Yardi do Azure AD, potřebujete e-učení Yardi tooadd hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8620c-124">tooconfigure hello integration of Yardi eLearning into Azure AD, you need tooadd Yardi eLearning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8620c-125">**e-učení Yardi tooadd z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8620c-125">**tooadd Yardi eLearning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8620c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8620c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8620c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8620c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8620c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8620c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8620c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8620c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8620c-133">Hello vyhledávacího pole zadejte **e-učení Yardi**.</span><span class="sxs-lookup"><span data-stu-id="8620c-133">In hello search box, type **Yardi eLearning**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_search.png)

5. <span data-ttu-id="8620c-135">Na panelu výsledků hello vyberte **e-učení Yardi**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8620c-135">In hello results panel, select **Yardi eLearning**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8620c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8620c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8620c-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s e-učení Yardi podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8620c-138">In this section, you configure and test Azure AD single sign-on with Yardi eLearning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8620c-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v e-učení Yardi je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8620c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Yardi eLearning is tooa user in Azure AD.</span></span> <span data-ttu-id="8620c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v e-učení Yardi musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8620c-140">In other words, a link relationship between an Azure AD user and hello related user in Yardi eLearning needs toobe established.</span></span>

<span data-ttu-id="8620c-141">V e-učení Yardi, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8620c-141">In Yardi eLearning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8620c-142">tooconfigure a testu Azure AD jednotné přihlašování s e-učení Yardi, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8620c-142">tooconfigure and test Azure AD single sign-on with Yardi eLearning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8620c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8620c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8620c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8620c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8620c-145">**[Vytvoření zkušebního uživatele e-učení Yardi](#creating-a-yardi-elearning-test-user)**  -toohave protějšek Britta Simon v e-učení Yardi, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8620c-145">**[Creating a Yardi eLearning test user](#creating-a-yardi-elearning-test-user)** - toohave a counterpart of Britta Simon in Yardi eLearning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8620c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8620c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8620c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8620c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8620c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8620c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8620c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Yardi e-učení.</span><span class="sxs-lookup"><span data-stu-id="8620c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Yardi eLearning application.</span></span>

<span data-ttu-id="8620c-150">**tooconfigure Azure AD jednotné přihlašování s e-učení Yardi, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8620c-150">**tooconfigure Azure AD single sign-on with Yardi eLearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="8620c-151">V portálu Azure, na hello hello **e-učení Yardi** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8620c-151">In hello Azure portal, on hello **Yardi eLearning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8620c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8620c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_samlbase.png)

3. <span data-ttu-id="8620c-155">Na hello **Yardi e-učení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8620c-155">On hello **Yardi eLearning Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_url.png)

    <span data-ttu-id="8620c-157">a.</span><span class="sxs-lookup"><span data-stu-id="8620c-157">a.</span></span> <span data-ttu-id="8620c-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.yardielearning.com/login`</span><span class="sxs-lookup"><span data-stu-id="8620c-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.yardielearning.com/login`</span></span>

    <span data-ttu-id="8620c-159">b.</span><span class="sxs-lookup"><span data-stu-id="8620c-159">b.</span></span> <span data-ttu-id="8620c-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.yardielearning.com/trust`</span><span class="sxs-lookup"><span data-stu-id="8620c-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.yardielearning.com/trust`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8620c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8620c-161">These values are not real.</span></span> <span data-ttu-id="8620c-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8620c-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8620c-163">Obraťte se na [tým podpory klienta e-učení Yardi](mailto:elearning@yardi.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8620c-163">Contact [Yardi eLearning Client support team](mailto:elearning@yardi.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="8620c-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="8620c-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_certificate.png) 

5. <span data-ttu-id="8620c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8620c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-yardielearning-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8620c-168">tooconfigure jednotného přihlašování na **e-učení Yardi** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory e-učení Yardi](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="8620c-168">tooconfigure single sign-on on **Yardi eLearning** side, you need toosend hello downloaded **Metadata XML** too[Yardi eLearning support team](mailto:elearning@yardi.com).</span></span> 

> [!TIP]
> <span data-ttu-id="8620c-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8620c-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8620c-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8620c-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8620c-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8620c-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8620c-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8620c-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="8620c-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8620c-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8620c-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8620c-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8620c-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8620c-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8620c-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8620c-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8620c-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8620c-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8620c-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8620c-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8620c-184">a.</span><span class="sxs-lookup"><span data-stu-id="8620c-184">a.</span></span> <span data-ttu-id="8620c-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8620c-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8620c-186">b.</span><span class="sxs-lookup"><span data-stu-id="8620c-186">b.</span></span> <span data-ttu-id="8620c-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8620c-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8620c-188">c.</span><span class="sxs-lookup"><span data-stu-id="8620c-188">c.</span></span> <span data-ttu-id="8620c-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8620c-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8620c-190">d.</span><span class="sxs-lookup"><span data-stu-id="8620c-190">d.</span></span> <span data-ttu-id="8620c-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8620c-191">Click **Create**.</span></span>
 
### <a name="creating-a-yardi-elearning-test-user"></a><span data-ttu-id="8620c-192">Vytvoření zkušebního uživatele Yardi e-učení</span><span class="sxs-lookup"><span data-stu-id="8620c-192">Creating a Yardi eLearning test user</span></span>

<span data-ttu-id="8620c-193">Hello cílem této části je toocreate volal Britta Simon v e-učení Yardi uživatele.</span><span class="sxs-lookup"><span data-stu-id="8620c-193">hello objective of this section is toocreate a user called Britta Simon in Yardi eLearning.</span></span> <span data-ttu-id="8620c-194">E-učení Yardi podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="8620c-194">Yardi eLearning supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="8620c-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="8620c-195">There is no action item for you in this section.</span></span> <span data-ttu-id="8620c-196">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Yardi e-učení.</span><span class="sxs-lookup"><span data-stu-id="8620c-196">A new user is created during an attempt tooaccess Yardi eLearning if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="8620c-197">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory e-učení Yardi](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="8620c-197">If you need toocreate a user manually, you need toocontact hello [Yardi eLearning support team](mailto:elearning@yardi.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8620c-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8620c-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8620c-199">V této části povolíte tak, že udělíte přístup tooYardi e-učení Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8620c-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYardi eLearning.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8620c-201">**tooassign Britta Simon tooYardi e-učení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8620c-201">**tooassign Britta Simon tooYardi eLearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="8620c-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8620c-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8620c-204">V seznamu aplikace hello vyberte **e-učení Yardi**.</span><span class="sxs-lookup"><span data-stu-id="8620c-204">In hello applications list, select **Yardi eLearning**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_app.png) 

3. <span data-ttu-id="8620c-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8620c-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8620c-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8620c-208">Click **Add** button.</span></span> <span data-ttu-id="8620c-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8620c-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8620c-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8620c-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8620c-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8620c-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8620c-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8620c-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8620c-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8620c-214">Testing single sign-on</span></span>

<span data-ttu-id="8620c-215">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8620c-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8620c-216">Po kliknutí na tlačítko hello Yardi e-učení dlaždice v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Yardi e-učení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8620c-216">When you click hello Yardi eLearning tile in hello Access Panel, you should get automatically signed-on tooyour Yardi eLearning application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8620c-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8620c-217">Additional resources</span></span>

* [<span data-ttu-id="8620c-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8620c-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8620c-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8620c-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_203.png

