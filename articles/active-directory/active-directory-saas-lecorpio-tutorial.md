---
title: 'Kurz: Azure Active Directory integrace s Lecorpio | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lecorpio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 963eb36678c589f942f63c7ab555161255324717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="c1e05-103">Kurz: Azure Active Directory integrace s Lecorpio</span><span class="sxs-lookup"><span data-stu-id="c1e05-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="c1e05-104">V tomto kurzu zjistíte, jak toointegrate Lecorpio s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c1e05-104">In this tutorial, you learn how toointegrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1e05-105">Integrace Lecorpio s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c1e05-105">Integrating Lecorpio with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c1e05-106">Můžete řídit ve službě Azure AD, který má přístup tooLecorpio</span><span class="sxs-lookup"><span data-stu-id="c1e05-106">You can control in Azure AD who has access tooLecorpio</span></span>
- <span data-ttu-id="c1e05-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLecorpio (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e05-107">You can enable your users tooautomatically get signed-on tooLecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c1e05-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c1e05-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c1e05-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1e05-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1e05-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1e05-110">Prerequisites</span></span>

<span data-ttu-id="c1e05-111">Integrace služby Azure AD s Lecorpio tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c1e05-111">tooconfigure Azure AD integration with Lecorpio, you need hello following items:</span></span>

- <span data-ttu-id="c1e05-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e05-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1e05-113">Lecorpio jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c1e05-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1e05-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1e05-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1e05-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c1e05-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1e05-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c1e05-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1e05-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1e05-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1e05-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c1e05-118">Scenario description</span></span>
<span data-ttu-id="c1e05-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1e05-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1e05-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c1e05-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1e05-121">Přidání Lecorpio z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1e05-121">Adding Lecorpio from hello gallery</span></span>
2. <span data-ttu-id="c1e05-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e05-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-hello-gallery"></a><span data-ttu-id="c1e05-123">Přidání Lecorpio z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c1e05-123">Adding Lecorpio from hello gallery</span></span>
<span data-ttu-id="c1e05-124">tooconfigure hello integrace Lecorpio do Azure AD, je nutné tooadd Lecorpio hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c1e05-124">tooconfigure hello integration of Lecorpio into Azure AD, you need tooadd Lecorpio from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c1e05-125">**tooadd Lecorpio z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e05-125">**tooadd Lecorpio from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e05-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c1e05-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c1e05-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c1e05-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c1e05-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1e05-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c1e05-133">Hello vyhledávacího pole zadejte **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-133">In hello search box, type **Lecorpio**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="c1e05-135">Na panelu výsledků hello vyberte **Lecorpio**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e05-135">In hello results panel, select **Lecorpio**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c1e05-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e05-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c1e05-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lecorpio podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c1e05-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c1e05-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Lecorpio je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1e05-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lecorpio is tooa user in Azure AD.</span></span> <span data-ttu-id="c1e05-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Lecorpio musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c1e05-140">In other words, a link relationship between an Azure AD user and hello related user in Lecorpio needs toobe established.</span></span>

<span data-ttu-id="c1e05-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="c1e05-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Lecorpio.</span></span>

<span data-ttu-id="c1e05-142">tooconfigure a testu Azure AD jednotné přihlašování s Lecorpio, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c1e05-142">tooconfigure and test Azure AD single sign-on with Lecorpio, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c1e05-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c1e05-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c1e05-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1e05-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1e05-145">**[Vytvoření zkušebního uživatele Lecorpio](#creating-a-lecorpio-test-user)**  -toohave protějšek Britta Simon v Lecorpio, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e05-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - toohave a counterpart of Britta Simon in Lecorpio that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1e05-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1e05-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1e05-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c1e05-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c1e05-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e05-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c1e05-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="c1e05-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="c1e05-150">**tooconfigure Azure AD jednotné přihlašování s Lecorpio, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e05-150">**tooconfigure Azure AD single sign-on with Lecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e05-151">V portálu Azure, na hello hello **Lecorpio** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-151">In hello Azure portal, on hello **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c1e05-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1e05-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="c1e05-155">Na hello **Lecorpio domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c1e05-155">On hello **Lecorpio Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="c1e05-157">a.</span><span class="sxs-lookup"><span data-stu-id="c1e05-157">a.</span></span> <span data-ttu-id="c1e05-158">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="c1e05-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="c1e05-159">b.</span><span class="sxs-lookup"><span data-stu-id="c1e05-159">b.</span></span> <span data-ttu-id="c1e05-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="c1e05-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c1e05-161">Tyto hodnoty nejsou skutečné hello.</span><span class="sxs-lookup"><span data-stu-id="c1e05-161">These values are not hello real.</span></span> <span data-ttu-id="c1e05-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c1e05-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="c1e05-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c1e05-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="c1e05-164">Obraťte se na [tým podpory Lecorpio klienta](mailto:info@lecorpio.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c1e05-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="c1e05-165">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c1e05-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="c1e05-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1e05-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c1e05-169">tooconfigure jednotného přihlašování na **Lecorpio** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory Lecorpio](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="c1e05-169">tooconfigure single sign-on on **Lecorpio** side, you need toosend hello downloaded **Metadata XML** too[Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="c1e05-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c1e05-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c1e05-171">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c1e05-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c1e05-172">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1e05-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c1e05-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1e05-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="c1e05-174">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c1e05-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c1e05-176">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e05-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e05-177">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c1e05-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c1e05-179">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c1e05-179">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c1e05-181">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1e05-181">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c1e05-183">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c1e05-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c1e05-185">a.</span><span class="sxs-lookup"><span data-stu-id="c1e05-185">a.</span></span> <span data-ttu-id="c1e05-186">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1e05-187">b.</span><span class="sxs-lookup"><span data-stu-id="c1e05-187">b.</span></span> <span data-ttu-id="c1e05-188">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c1e05-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c1e05-189">c.</span><span class="sxs-lookup"><span data-stu-id="c1e05-189">c.</span></span> <span data-ttu-id="c1e05-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c1e05-191">d.</span><span class="sxs-lookup"><span data-stu-id="c1e05-191">d.</span></span> <span data-ttu-id="c1e05-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="c1e05-193">Vytvoření zkušebního uživatele Lecorpio</span><span class="sxs-lookup"><span data-stu-id="c1e05-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="c1e05-194">V této části vytvoříte volal Britta Simon v Lecorpio uživatele.</span><span class="sxs-lookup"><span data-stu-id="c1e05-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="c1e05-195">Obraťte se na [tým podpory Lecorpio klienta](mailto:info@lecorpio.com) tooadd hello uživatele v hello Lecorpio aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e05-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) tooadd hello users in hello Lecorpio application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c1e05-196">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c1e05-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c1e05-197">V této části povolíte tak, že udělíte přístup tooLecorpio toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c1e05-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLecorpio.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c1e05-199">**tooassign Britta Simon tooLecorpio, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c1e05-199">**tooassign Britta Simon tooLecorpio, perform hello following steps:**</span></span>

1. <span data-ttu-id="c1e05-200">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c1e05-202">V seznamu aplikace hello vyberte **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-202">In hello applications list, select **Lecorpio**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="c1e05-204">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c1e05-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c1e05-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1e05-206">Click **Add** button.</span></span> <span data-ttu-id="c1e05-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1e05-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c1e05-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c1e05-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c1e05-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1e05-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1e05-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c1e05-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c1e05-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c1e05-212">Testing single sign-on</span></span>

<span data-ttu-id="c1e05-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c1e05-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c1e05-214">Když kliknete na dlaždici Lecorpio hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Lecorpio aplikace.</span><span class="sxs-lookup"><span data-stu-id="c1e05-214">When you click hello Lecorpio tile in hello Access Panel, you should get automatically signed-on tooyour Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1e05-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c1e05-215">Additional resources</span></span>

* [<span data-ttu-id="c1e05-216">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c1e05-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1e05-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c1e05-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

