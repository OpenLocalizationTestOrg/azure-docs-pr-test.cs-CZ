---
title: 'Kurz: Azure Active Directory integrace s 123ContactForm | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a 123ContactForm."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a><span data-ttu-id="4e835-103">Kurz: Azure Active Directory integrace s 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="4e835-103">Tutorial: Azure Active Directory integration with 123ContactForm</span></span>

<span data-ttu-id="4e835-104">V tomto kurzu zjistíte, jak toointegrate 123ContactForm s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4e835-104">In this tutorial, you learn how toointegrate 123ContactForm with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4e835-105">Integrace 123ContactForm s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4e835-105">Integrating 123ContactForm with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4e835-106">Můžete řídit ve službě Azure AD, který má přístup too123ContactForm</span><span class="sxs-lookup"><span data-stu-id="4e835-106">You can control in Azure AD who has access too123ContactForm</span></span>
- <span data-ttu-id="4e835-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného too123ContactForm (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e835-107">You can enable your users tooautomatically get signed-on too123ContactForm (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4e835-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4e835-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4e835-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4e835-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e835-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4e835-110">Prerequisites</span></span>

<span data-ttu-id="4e835-111">Integrace služby Azure AD s 123ContactForm tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4e835-111">tooconfigure Azure AD integration with 123ContactForm, you need hello following items:</span></span>

- <span data-ttu-id="4e835-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e835-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4e835-113">123ContactForm jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4e835-113">A 123ContactForm single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4e835-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e835-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4e835-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4e835-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4e835-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4e835-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4e835-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e835-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4e835-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4e835-118">Scenario description</span></span>
<span data-ttu-id="4e835-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4e835-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4e835-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4e835-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4e835-121">Přidání 123ContactForm z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4e835-121">Adding 123ContactForm from hello gallery</span></span>
2. <span data-ttu-id="4e835-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e835-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-123contactform-from-hello-gallery"></a><span data-ttu-id="4e835-123">Přidání 123ContactForm z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4e835-123">Adding 123ContactForm from hello gallery</span></span>
<span data-ttu-id="4e835-124">tooconfigure hello integrace 123ContactForm do Azure AD, je nutné tooadd 123ContactForm hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4e835-124">tooconfigure hello integration of 123ContactForm into Azure AD, you need tooadd 123ContactForm from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4e835-125">**tooadd 123ContactForm z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4e835-125">**tooadd 123ContactForm from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e835-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4e835-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4e835-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4e835-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4e835-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4e835-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4e835-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e835-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4e835-133">Hello vyhledávacího pole zadejte **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="4e835-133">In hello search box, type **123ContactForm**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. <span data-ttu-id="4e835-135">Na panelu výsledků hello vyberte **123ContactForm**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e835-135">In hello results panel, select **123ContactForm**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4e835-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e835-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4e835-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s 123ContactForm podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="4e835-138">In this section, you configure and test Azure AD single sign-on with 123ContactForm based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4e835-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v 123ContactForm je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e835-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 123ContactForm is tooa user in Azure AD.</span></span> <span data-ttu-id="4e835-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v 123ContactForm musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="4e835-140">In other words, a link relationship between an Azure AD user and hello related user in 123ContactForm needs toobe established.</span></span>

<span data-ttu-id="4e835-141">V 123ContactForm, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="4e835-141">In 123ContactForm, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4e835-142">tooconfigure a testu Azure AD jednotné přihlašování s 123ContactForm, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="4e835-142">tooconfigure and test Azure AD single sign-on with 123ContactForm, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4e835-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4e835-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4e835-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e835-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4e835-145">**[Vytvoření zkušebního uživatele 123ContactForm](#creating-a-123contactform-test-user)**  -toohave protějšek Britta Simon v 123ContactForm, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4e835-145">**[Creating a 123ContactForm test user](#creating-a-123contactform-test-user)** - toohave a counterpart of Britta Simon in 123ContactForm that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4e835-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4e835-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4e835-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="4e835-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4e835-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e835-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4e835-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci 123ContactForm.</span><span class="sxs-lookup"><span data-stu-id="4e835-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 123ContactForm application.</span></span>

<span data-ttu-id="4e835-150">**tooconfigure Azure AD jednotné přihlašování s 123ContactForm, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4e835-150">**tooconfigure Azure AD single sign-on with 123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e835-151">V portálu Azure, na hello hello **123ContactForm** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4e835-151">In hello Azure portal, on hello **123ContactForm** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4e835-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4e835-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. <span data-ttu-id="4e835-155">Na hello **123ContactForm domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4e835-155">On hello **123ContactForm Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url1.png)

    <span data-ttu-id="4e835-157">a.</span><span class="sxs-lookup"><span data-stu-id="4e835-157">a.</span></span> <span data-ttu-id="4e835-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span><span class="sxs-lookup"><span data-stu-id="4e835-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`</span></span>

    <span data-ttu-id="4e835-159">b.</span><span class="sxs-lookup"><span data-stu-id="4e835-159">b.</span></span> <span data-ttu-id="4e835-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span><span class="sxs-lookup"><span data-stu-id="4e835-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`</span></span>

4. <span data-ttu-id="4e835-161">Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4e835-161">If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url2.png)

    <span data-ttu-id="4e835-163">a.</span><span class="sxs-lookup"><span data-stu-id="4e835-163">a.</span></span> <span data-ttu-id="4e835-164">Klikněte na tlačítko hello **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="4e835-164">Click hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="4e835-165">b.</span><span class="sxs-lookup"><span data-stu-id="4e835-165">b.</span></span> <span data-ttu-id="4e835-166">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span><span class="sxs-lookup"><span data-stu-id="4e835-166">In hello **Sign On URL** textbox, type a URL as: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4e835-167">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="4e835-167">These values are not real.</span></span> <span data-ttu-id="4e835-168">Tooupdate budete potřebovat tyto hodnoty z skutečné adresy URL a identifikátor, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="4e835-168">You'll need tooupdate these value from actual URLs and Identifier which is explained later in hello tutorial.</span></span>
    
5. <span data-ttu-id="4e835-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4e835-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. <span data-ttu-id="4e835-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e835-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="4e835-173">tooconfigure jednotného přihlašování na **123ContactForm** straně, přejděte příliš[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e835-173">tooconfigure single sign-on on **123ContactForm** side, go too[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) and perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    <span data-ttu-id="4e835-175">a.</span><span class="sxs-lookup"><span data-stu-id="4e835-175">a.</span></span> <span data-ttu-id="4e835-176">V hello **e-mailu** textovému poli, typ hello e-mail uživatele jednofaktorovému hello</span><span class="sxs-lookup"><span data-stu-id="4e835-176">In hello **Email** textbox, type hello email of hello user i.e</span></span> <span data-ttu-id="4e835-177">**BrittaSimon@Contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="4e835-177">**BrittaSimon@Contoso.com**.</span></span>

    <span data-ttu-id="4e835-178">b.</span><span class="sxs-lookup"><span data-stu-id="4e835-178">b.</span></span> <span data-ttu-id="4e835-179">Klikněte na tlačítko **nahrát** a procházet hello soubor XML s metadaty souboru, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e835-179">Click **Upload** and browse hello Metadata XML file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="4e835-180">c.</span><span class="sxs-lookup"><span data-stu-id="4e835-180">c.</span></span> <span data-ttu-id="4e835-181">Klikněte na tlačítko **odeslání formuláře**.</span><span class="sxs-lookup"><span data-stu-id="4e835-181">Click **SUBMIT FORM**.</span></span>

8. <span data-ttu-id="4e835-182">Na hello **nakonfigurovat nastavení aplikace služby Microsoft Azure AD jednotné přihlašování –** provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e835-182">On hello **Microsoft Azure AD - Single sign-on - Configure App Settings** perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/url3.png)

    <span data-ttu-id="4e835-184">a.</span><span class="sxs-lookup"><span data-stu-id="4e835-184">a.</span></span> <span data-ttu-id="4e835-185">Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, kopie hello **IDENTIFIKÁTOR** instance a vložte ji v **identifikátor** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e835-185">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **IDENTIFIER** value for your instance and paste it in **Identifier** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>
    
    <span data-ttu-id="4e835-186">b.</span><span class="sxs-lookup"><span data-stu-id="4e835-186">b.</span></span> <span data-ttu-id="4e835-187">Pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, kopie hello **adresa URL odpovědi** instance a vložte ji v **adresa URL odpovědi** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e835-187">If you wish tooconfigure hello application in **IDP initiated mode**, copy hello **REPLY URL** value for your instance and paste it in **Reply URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="4e835-188">c.</span><span class="sxs-lookup"><span data-stu-id="4e835-188">c.</span></span> <span data-ttu-id="4e835-189">Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, kopie hello **ON PŘIHLAŠOVACÍ adresa URL** instance a vložte ji v **přihlašovací adresa URL** textového pole v **123ContactForm domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4e835-189">If you wish tooconfigure hello application in **SP initiated mode**, copy hello **SIGN ON URL** value for your instance and paste it in **Sign On URL** textbox in **123ContactForm Domain and URLs** section on Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="4e835-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="4e835-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4e835-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="4e835-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4e835-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4e835-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4e835-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e835-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="4e835-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="4e835-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4e835-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4e835-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e835-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4e835-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4e835-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4e835-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4e835-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="4e835-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4e835-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4e835-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4e835-205">a.</span><span class="sxs-lookup"><span data-stu-id="4e835-205">a.</span></span> <span data-ttu-id="4e835-206">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4e835-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4e835-207">b.</span><span class="sxs-lookup"><span data-stu-id="4e835-207">b.</span></span> <span data-ttu-id="4e835-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4e835-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4e835-209">c.</span><span class="sxs-lookup"><span data-stu-id="4e835-209">c.</span></span> <span data-ttu-id="4e835-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4e835-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4e835-211">d.</span><span class="sxs-lookup"><span data-stu-id="4e835-211">d.</span></span> <span data-ttu-id="4e835-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4e835-212">Click **Create**.</span></span>
 
### <a name="creating-a-123contactform-test-user"></a><span data-ttu-id="4e835-213">Vytvoření zkušebního uživatele 123ContactForm</span><span class="sxs-lookup"><span data-stu-id="4e835-213">Creating a 123ContactForm test user</span></span>

<span data-ttu-id="4e835-214">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele budou vytvořeny v hello aplikace automaticky.</span><span class="sxs-lookup"><span data-stu-id="4e835-214">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4e835-215">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="4e835-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4e835-216">V této části povolíte tak, že udělíte přístup too123ContactForm toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4e835-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too123ContactForm.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4e835-218">**tooassign Britta Simon too123ContactForm, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4e835-218">**tooassign Britta Simon too123ContactForm, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e835-219">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4e835-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4e835-221">V seznamu aplikace hello vyberte **123ContactForm**.</span><span class="sxs-lookup"><span data-stu-id="4e835-221">In hello applications list, select **123ContactForm**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. <span data-ttu-id="4e835-223">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4e835-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4e835-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4e835-225">Click **Add** button.</span></span> <span data-ttu-id="4e835-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e835-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4e835-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="4e835-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4e835-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e835-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e835-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4e835-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4e835-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4e835-231">Testing single sign-on</span></span>

<span data-ttu-id="4e835-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4e835-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4e835-233">Když kliknete na dlaždici 123ContactForm hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour 123ContactForm aplikace.</span><span class="sxs-lookup"><span data-stu-id="4e835-233">When you click hello 123ContactForm tile in hello Access Panel, you should get automatically signed-on tooyour 123ContactForm application.</span></span>
<span data-ttu-id="4e835-234">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4e835-234">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e835-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4e835-235">Additional resources</span></span>

* [<span data-ttu-id="4e835-236">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e835-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4e835-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4e835-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

