---
title: 'Kurz: Azure Active Directory integrace s lidmi | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a osoby."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: d540c31867c92c4dc09db9c0833f8a8a7c02b371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a><span data-ttu-id="ff7e3-103">Kurz: Azure Active Directory integrace s uživateli</span><span class="sxs-lookup"><span data-stu-id="ff7e3-103">Tutorial: Azure Active Directory integration with People</span></span>

<span data-ttu-id="ff7e3-104">V tomto kurzu zjistíte, jak uživatelé toointegrate s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ff7e3-104">In this tutorial, you learn how toointegrate People with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff7e3-105">Integrace osoby s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-105">Integrating People with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ff7e3-106">Můžete řídit ve službě Azure AD, který má přístup tooPeople</span><span class="sxs-lookup"><span data-stu-id="ff7e3-106">You can control in Azure AD who has access tooPeople</span></span>
- <span data-ttu-id="ff7e3-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPeople (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff7e3-107">You can enable your users tooautomatically get signed-on tooPeople (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff7e3-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ff7e3-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ff7e3-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff7e3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff7e3-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ff7e3-110">Prerequisites</span></span>

<span data-ttu-id="ff7e3-111">tooconfigure integrace Azure AD s lidmi, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-111">tooconfigure Azure AD integration with People, you need hello following items:</span></span>

- <span data-ttu-id="ff7e3-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff7e3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ff7e3-113">Osoby jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ff7e3-113">A People single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff7e3-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff7e3-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff7e3-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff7e3-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff7e3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff7e3-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ff7e3-118">Scenario description</span></span>
<span data-ttu-id="ff7e3-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff7e3-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff7e3-121">Přidání uživatelů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ff7e3-121">Adding People from hello gallery</span></span>
2. <span data-ttu-id="ff7e3-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff7e3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-people-from-hello-gallery"></a><span data-ttu-id="ff7e3-123">Přidání uživatelů z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ff7e3-123">Adding People from hello gallery</span></span>
<span data-ttu-id="ff7e3-124">tooconfigure hello integrace lidí do Azure AD, je nutné tooadd osoby z hello Galerie tooyour seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-124">tooconfigure hello integration of People into Azure AD, you need tooadd People from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ff7e3-125">**tooadd osoby z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff7e3-125">**tooadd People from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff7e3-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff7e3-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ff7e3-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ff7e3-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ff7e3-133">Hello vyhledávacího pole zadejte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-133">In hello search box, type **People**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_search.png)

5. <span data-ttu-id="ff7e3-135">Na panelu výsledků hello vyberte **osoby**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-135">In hello results panel, select **People**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff7e3-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff7e3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff7e3-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s uživateli na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ff7e3-138">In this section, you configure and test Azure AD single sign-on with People based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ff7e3-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v osoby je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in People is tooa user in Azure AD.</span></span> <span data-ttu-id="ff7e3-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v osoby musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-140">In other words, a link relationship between an Azure AD user and hello related user in People needs toobe established.</span></span>

<span data-ttu-id="ff7e3-141">V osoby, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-141">In People, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ff7e3-142">tooconfigure a testování Azure AD jednotné přihlašování s lidmi, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-142">tooconfigure and test Azure AD single sign-on with People, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ff7e3-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ff7e3-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff7e3-145">**[Vytvoření zkušebního uživatele osoby](#creating-a-people-test-user)**  -toohave protějšek Britta Simon v osoby, které je propojené toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-145">**[Creating a People test user](#creating-a-people-test-user)** - toohave a counterpart of Britta Simon in People that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff7e3-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff7e3-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff7e3-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff7e3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff7e3-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci osoby.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your People application.</span></span>

<span data-ttu-id="ff7e3-150">**tooconfigure Azure AD jednotné přihlašování s lidmi, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff7e3-150">**tooconfigure Azure AD single sign-on with People, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff7e3-151">V portálu Azure, na hello hello **osoby** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-151">In hello Azure portal, on hello **People** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ff7e3-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_samlbase.png)

3. <span data-ttu-id="ff7e3-155">Na hello **uživatelé domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-155">On hello **People Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_url.png)

    <span data-ttu-id="ff7e3-157">a.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-157">a.</span></span> <span data-ttu-id="ff7e3-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.peoplehr.com/`</span><span class="sxs-lookup"><span data-stu-id="ff7e3-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.peoplehr.com/`</span></span>

    <span data-ttu-id="ff7e3-159">b.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-159">b.</span></span> <span data-ttu-id="ff7e3-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.peoplehr.com`</span><span class="sxs-lookup"><span data-stu-id="ff7e3-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.peoplehr.com`</span></span>

    <span data-ttu-id="ff7e3-161">c.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-161">c.</span></span> <span data-ttu-id="ff7e3-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span><span class="sxs-lookup"><span data-stu-id="ff7e3-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff7e3-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-163">These values are not real.</span></span> <span data-ttu-id="ff7e3-164">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ff7e3-165">Obraťte se na [tým podpory osoby klienta](mailto:customerservices@peoplehr.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-165">Contact [People Client support team](mailto:customerservices@peoplehr.com) tooget these values.</span></span>

5. <span data-ttu-id="ff7e3-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-166">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_certificate.png) 

6. <span data-ttu-id="ff7e3-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="ff7e3-170">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta osoby toosign na tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-170">tooget SSO configured for your application, you need toosign-on tooyour People tenant as an administrator.</span></span>
   
8. <span data-ttu-id="ff7e3-171">V nabídce hello na levé straně hello, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-171">In hello menu on hello left side, click **Settings**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

9. <span data-ttu-id="ff7e3-173">Klikněte na tlačítko **společnosti**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-173">Click **Company**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

10. <span data-ttu-id="ff7e3-175">Na hello **nahrávání, jednotné přihlašování, SAML metadata souboru**, klikněte na tlačítko **Procházet** tooupload hello stáhnout soubor metadat.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-175">On hello **Upload 'Single Sign On' SAML meta-data file**, click **Browse** tooupload hello downloaded metadata file.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)

> [!TIP]
> <span data-ttu-id="ff7e3-177">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ff7e3-177">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ff7e3-178">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-178">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ff7e3-179">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff7e3-179">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff7e3-180">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff7e3-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff7e3-181">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-181">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ff7e3-183">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff7e3-183">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff7e3-184">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-184">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff7e3-186">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-186">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff7e3-188">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-188">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff7e3-190">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ff7e3-190">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff7e3-192">a.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-192">a.</span></span> <span data-ttu-id="ff7e3-193">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-193">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff7e3-194">b.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-194">b.</span></span> <span data-ttu-id="ff7e3-195">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-195">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff7e3-196">c.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-196">c.</span></span> <span data-ttu-id="ff7e3-197">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-197">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ff7e3-198">d.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-198">d.</span></span> <span data-ttu-id="ff7e3-199">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-199">Click **Create**.</span></span>
 
### <a name="creating-a-people-test-user"></a><span data-ttu-id="ff7e3-200">Vytvoření zkušebního uživatele osoby</span><span class="sxs-lookup"><span data-stu-id="ff7e3-200">Creating a People test user</span></span>

<span data-ttu-id="ff7e3-201">V této části vytvoříte uživatele volal Britta Simon v osoby.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-201">In this section, you create a user called Britta Simon in People.</span></span> <span data-ttu-id="ff7e3-202">Práce s [tým podpory osoby klienta](mailto:customerservices@peoplehr.com) pro přidání uživatelů hello hello osoby platformy.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-202">Work with [People Client support team](mailto:customerservices@peoplehr.com) to add hello users in hello People platform.</span></span> <span data-ttu-id="ff7e3-203">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-203">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ff7e3-204">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ff7e3-204">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ff7e3-205">V této části povolíte tak, že udělíte přístup tooPeople toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-205">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPeople.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ff7e3-207">**tooassign Britta Simon tooPeople, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ff7e3-207">**tooassign Britta Simon tooPeople, perform hello following steps:**</span></span>

1. <span data-ttu-id="ff7e3-208">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-208">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ff7e3-210">V seznamu aplikace hello vyberte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-210">In hello applications list, select **People**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_app.png) 

3. <span data-ttu-id="ff7e3-212">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-212">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ff7e3-214">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-214">Click **Add** button.</span></span> <span data-ttu-id="ff7e3-215">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-215">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ff7e3-217">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-217">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ff7e3-218">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-218">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff7e3-219">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-219">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff7e3-220">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ff7e3-220">Testing single sign-on</span></span>

<span data-ttu-id="ff7e3-221">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-221">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="ff7e3-222">Po kliknutí na tlačítko hello osoby dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour osoby aplikace.</span><span class="sxs-lookup"><span data-stu-id="ff7e3-222">When you click hello People tile in hello Access Panel, you should get automatically signed-on tooyour People application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff7e3-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ff7e3-223">Additional resources</span></span>

* [<span data-ttu-id="ff7e3-224">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff7e3-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff7e3-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff7e3-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-people-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png

