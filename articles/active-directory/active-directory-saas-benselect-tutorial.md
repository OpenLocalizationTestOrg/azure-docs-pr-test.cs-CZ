---
title: 'Kurz: Azure Active Directory integrace s BenSelect | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a BenSelect."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c3705da337bf8f6e76de58cd21c5b047c8f5e12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="c7975-103">Kurz: Azure Active Directory integrace s BenSelect</span><span class="sxs-lookup"><span data-stu-id="c7975-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="c7975-104">V tomto kurzu zjistíte, jak toointegrate BenSelect s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c7975-104">In this tutorial, you learn how toointegrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c7975-105">Integrace BenSelect s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c7975-105">Integrating BenSelect with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c7975-106">Můžete řídit ve službě Azure AD, který má přístup tooBenSelect</span><span class="sxs-lookup"><span data-stu-id="c7975-106">You can control in Azure AD who has access tooBenSelect</span></span>
- <span data-ttu-id="c7975-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBenSelect (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7975-107">You can enable your users tooautomatically get signed-on tooBenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c7975-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c7975-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c7975-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c7975-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7975-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7975-110">Prerequisites</span></span>

<span data-ttu-id="c7975-111">Integrace služby Azure AD s BenSelect tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c7975-111">tooconfigure Azure AD integration with BenSelect, you need hello following items:</span></span>

- <span data-ttu-id="c7975-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7975-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c7975-113">BenSelect jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c7975-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c7975-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c7975-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c7975-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c7975-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c7975-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c7975-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c7975-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7975-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c7975-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c7975-118">Scenario description</span></span>
<span data-ttu-id="c7975-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c7975-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c7975-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c7975-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c7975-121">Přidání BenSelect z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c7975-121">Adding BenSelect from hello gallery</span></span>
2. <span data-ttu-id="c7975-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7975-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-hello-gallery"></a><span data-ttu-id="c7975-123">Přidání BenSelect z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c7975-123">Adding BenSelect from hello gallery</span></span>
<span data-ttu-id="c7975-124">tooconfigure hello integrace BenSelect do Azure AD, je nutné tooadd BenSelect hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c7975-124">tooconfigure hello integration of BenSelect into Azure AD, you need tooadd BenSelect from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c7975-125">**tooadd BenSelect z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c7975-125">**tooadd BenSelect from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7975-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c7975-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c7975-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c7975-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c7975-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c7975-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c7975-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7975-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c7975-133">Hello vyhledávacího pole zadejte **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="c7975-133">In hello search box, type **BenSelect**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="c7975-135">Na panelu výsledků hello vyberte **BenSelect**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7975-135">In hello results panel, select **BenSelect**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c7975-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7975-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c7975-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BenSelect podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="c7975-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c7975-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v BenSelect je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c7975-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BenSelect is tooa user in Azure AD.</span></span> <span data-ttu-id="c7975-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v BenSelect musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c7975-140">In other words, a link relationship between an Azure AD user and hello related user in BenSelect needs toobe established.</span></span>

<span data-ttu-id="c7975-141">V BenSelect, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c7975-141">In BenSelect, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c7975-142">tooconfigure a testu Azure AD jednotné přihlašování s BenSelect, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c7975-142">tooconfigure and test Azure AD single sign-on with BenSelect, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c7975-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c7975-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c7975-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c7975-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c7975-145">**[Vytvoření zkušebního uživatele BenSelect](#creating-a-benselect-test-user)**  -toohave protějšek Britta Simon v BenSelect, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c7975-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - toohave a counterpart of Britta Simon in BenSelect that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c7975-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7975-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c7975-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c7975-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c7975-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7975-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c7975-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci BenSelect.</span><span class="sxs-lookup"><span data-stu-id="c7975-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="c7975-150">**tooconfigure Azure AD jednotné přihlašování s BenSelect, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c7975-150">**tooconfigure Azure AD single sign-on with BenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7975-151">V portálu Azure, na hello hello **BenSelect** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c7975-151">In hello Azure portal, on hello **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c7975-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7975-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="c7975-155">Na hello **BenSelect domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c7975-155">On hello **BenSelect Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="c7975-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="c7975-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c7975-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="c7975-158">This value is not real.</span></span> <span data-ttu-id="c7975-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="c7975-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="c7975-160">Obraťte se na [tým podpory BenSelect](mailto:support@selerix.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c7975-160">Contact [BenSelect support team](mailto:support@selerix.com) tooget this value.</span></span>
 
4. <span data-ttu-id="c7975-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c7975-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="c7975-163">Aplikace BenSelect očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="c7975-163">BenSelect application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c7975-164">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c7975-164">Configure hello following claims for this application.</span></span> <span data-ttu-id="c7975-165">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7975-165">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="c7975-166">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="c7975-166">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="c7975-168">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="c7975-168">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="c7975-169">a.</span><span class="sxs-lookup"><span data-stu-id="c7975-169">a.</span></span> <span data-ttu-id="c7975-170">V hello **uživatelský identifikátor** rozevíracího seznamu vyberte **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="c7975-170">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="c7975-171">b.</span><span class="sxs-lookup"><span data-stu-id="c7975-171">b.</span></span> <span data-ttu-id="c7975-172">V hello **e-mailu** rozevíracího seznamu vyberte **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="c7975-172">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="c7975-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7975-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c7975-175">Na hello **BenSelect konfigurace** klikněte na tlačítko **konfigurace BenSelect** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c7975-175">On hello **BenSelect Configuration** section, click **Configure BenSelect** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c7975-176">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c7975-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="c7975-178">tooconfigure jednotného přihlašování na **BenSelect** straně, je nutné stáhnout hello toosend **Certificate(Raw)** a **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby**příliš[tým podpory BenSelect](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="c7975-178">tooconfigure single sign-on on **BenSelect** side, you need toosend hello downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="c7975-179">Je třeba toomention vyžadující algoritmus hello SHA256 (SHA1 není podporována.) této integrace tooset hello jednotného přihlašování na příslušný server hello jako app2101 atd.</span><span class="sxs-lookup"><span data-stu-id="c7975-179">You need toomention that this integration requires hello SHA256 algorithm (SHA1 is not supported) tooset hello SSO on hello appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="c7975-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c7975-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c7975-181">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c7975-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c7975-182">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c7975-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c7975-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c7975-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="c7975-184">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c7975-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c7975-186">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c7975-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7975-187">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c7975-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c7975-189">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c7975-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c7975-191">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c7975-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c7975-193">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7975-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c7975-195">a.</span><span class="sxs-lookup"><span data-stu-id="c7975-195">a.</span></span> <span data-ttu-id="c7975-196">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c7975-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c7975-197">b.</span><span class="sxs-lookup"><span data-stu-id="c7975-197">b.</span></span> <span data-ttu-id="c7975-198">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c7975-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c7975-199">c.</span><span class="sxs-lookup"><span data-stu-id="c7975-199">c.</span></span> <span data-ttu-id="c7975-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c7975-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c7975-201">d.</span><span class="sxs-lookup"><span data-stu-id="c7975-201">d.</span></span> <span data-ttu-id="c7975-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7975-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="c7975-203">Vytvoření zkušebního uživatele BenSelect</span><span class="sxs-lookup"><span data-stu-id="c7975-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="c7975-204">Hello cílem této části je toocreate volal Britta Simon v BenSelect uživatele.</span><span class="sxs-lookup"><span data-stu-id="c7975-204">hello objective of this section is toocreate a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="c7975-205">Práce s [tým podpory BenSelect](mailto:support@selerix.com) tooadd hello uživatele v hello BenSelect účtu.</span><span class="sxs-lookup"><span data-stu-id="c7975-205">Work with [BenSelect support team](mailto:support@selerix.com) tooadd hello users in hello BenSelect account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c7975-206">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c7975-206">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c7975-207">V této části povolíte tak, že udělíte přístup tooBenSelect toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c7975-207">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBenSelect.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c7975-209">**tooassign Britta Simon tooBenSelect, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c7975-209">**tooassign Britta Simon tooBenSelect, perform hello following steps:**</span></span>

1. <span data-ttu-id="c7975-210">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c7975-210">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c7975-212">V seznamu aplikace hello vyberte **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="c7975-212">In hello applications list, select **BenSelect**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="c7975-214">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c7975-214">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c7975-216">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c7975-216">Click **Add** button.</span></span> <span data-ttu-id="c7975-217">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c7975-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c7975-219">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c7975-219">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c7975-220">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c7975-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c7975-221">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c7975-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c7975-222">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c7975-222">Testing single sign-on</span></span>

<span data-ttu-id="c7975-223">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c7975-223">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="c7975-224">Když kliknete na dlaždici BenSelect hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour BenSelect aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7975-224">When you click hello BenSelect tile in hello Access Panel, you should get automatically signed-on tooyour BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c7975-225">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c7975-225">Additional resources</span></span>

* [<span data-ttu-id="c7975-226">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c7975-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c7975-227">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c7975-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

