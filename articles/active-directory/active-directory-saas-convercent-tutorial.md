---
title: 'Kurz: Azure Active Directory integrace s Convercent | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Convercent."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: e6d09d7de52779dcf05e80215df9369ebc2ed06d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a><span data-ttu-id="efff6-103">Kurz: Azure Active Directory integrace s Convercent</span><span class="sxs-lookup"><span data-stu-id="efff6-103">Tutorial: Azure Active Directory integration with Convercent</span></span>

<span data-ttu-id="efff6-104">V tomto kurzu zjistíte, jak toointegrate Convercent s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="efff6-104">In this tutorial, you learn how toointegrate Convercent with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="efff6-105">Integrace Convercent s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="efff6-105">Integrating Convercent with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="efff6-106">Můžete řídit ve službě Azure AD, který má přístup tooConvercent</span><span class="sxs-lookup"><span data-stu-id="efff6-106">You can control in Azure AD who has access tooConvercent</span></span>
- <span data-ttu-id="efff6-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooConvercent (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="efff6-107">You can enable your users tooautomatically get signed-on tooConvercent (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="efff6-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="efff6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="efff6-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="efff6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efff6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="efff6-110">Prerequisites</span></span>

<span data-ttu-id="efff6-111">Integrace služby Azure AD s Convercent tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="efff6-111">tooconfigure Azure AD integration with Convercent, you need hello following items:</span></span>

- <span data-ttu-id="efff6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="efff6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="efff6-113">Convercent jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="efff6-113">A Convercent single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="efff6-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="efff6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="efff6-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="efff6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="efff6-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="efff6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="efff6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="efff6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="efff6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="efff6-118">Scenario description</span></span>
<span data-ttu-id="efff6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="efff6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="efff6-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="efff6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="efff6-121">Přidání Convercent z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="efff6-121">Adding Convercent from hello gallery</span></span>
2. <span data-ttu-id="efff6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="efff6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-convercent-from-hello-gallery"></a><span data-ttu-id="efff6-123">Přidání Convercent z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="efff6-123">Adding Convercent from hello gallery</span></span>
<span data-ttu-id="efff6-124">tooconfigure hello integrace Convercent do Azure AD, je nutné tooadd Convercent hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="efff6-124">tooconfigure hello integration of Convercent into Azure AD, you need tooadd Convercent from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="efff6-125">**tooadd Convercent z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="efff6-125">**tooadd Convercent from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="efff6-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="efff6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="efff6-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="efff6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="efff6-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="efff6-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="efff6-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efff6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="efff6-133">Hello vyhledávacího pole zadejte **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="efff6-133">In hello search box, type **Convercent**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_search.png)

5. <span data-ttu-id="efff6-135">Na panelu výsledků hello vyberte **Convercent**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="efff6-135">In hello results panel, select **Convercent**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="efff6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="efff6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="efff6-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Convercent podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="efff6-138">In this section, you configure and test Azure AD single sign-on with Convercent based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="efff6-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Convercent je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="efff6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Convercent is tooa user in Azure AD.</span></span> <span data-ttu-id="efff6-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Convercent musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="efff6-140">In other words, a link relationship between an Azure AD user and hello related user in Convercent needs toobe established.</span></span>

<span data-ttu-id="efff6-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Convercent.</span><span class="sxs-lookup"><span data-stu-id="efff6-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Convercent.</span></span>

<span data-ttu-id="efff6-142">tooconfigure a testu Azure AD jednotné přihlašování s Convercent, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="efff6-142">tooconfigure and test Azure AD single sign-on with Convercent, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="efff6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="efff6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="efff6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="efff6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="efff6-145">**[Vytvoření zkušebního uživatele Convercent](#creating-a-convercent-test-user)**  -toohave protějšek Britta Simon v Convercent, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="efff6-145">**[Creating a Convercent test user](#creating-a-convercent-test-user)** - toohave a counterpart of Britta Simon in Convercent that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="efff6-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="efff6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="efff6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="efff6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="efff6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="efff6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="efff6-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Convercent.</span><span class="sxs-lookup"><span data-stu-id="efff6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Convercent application.</span></span>

<span data-ttu-id="efff6-150">**tooconfigure Azure AD jednotné přihlašování s Convercent, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="efff6-150">**tooconfigure Azure AD single sign-on with Convercent, perform hello following steps:**</span></span>

1. <span data-ttu-id="efff6-151">V portálu Azure, na hello hello **Convercent** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="efff6-151">In hello Azure portal, on hello **Convercent** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="efff6-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="efff6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_samlbase.png)

3. <span data-ttu-id="efff6-155">Na hello **Convercent domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="efff6-155">On hello **Convercent Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url.png)

    <span data-ttu-id="efff6-157">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="efff6-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.convercent.com/`</span></span>
 
4. <span data-ttu-id="efff6-158">Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, na hello **Convercent domény a adresy URL** části provést následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="efff6-158">If you wish tooconfigure hello application in **SP initiated mode**, on hello **Convercent Domain and URLs** section perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url1.png)

     <span data-ttu-id="efff6-160">a.</span><span class="sxs-lookup"><span data-stu-id="efff6-160">a.</span></span> <span data-ttu-id="efff6-161">Klikněte na tlačítko **"Zobrazit rozšířené nastavení adresy URL."**</span><span class="sxs-lookup"><span data-stu-id="efff6-161">Click **"Show advanced URL settings."**</span></span> 

     <span data-ttu-id="efff6-162">b.</span><span class="sxs-lookup"><span data-stu-id="efff6-162">b.</span></span> <span data-ttu-id="efff6-163">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="efff6-163">In hello **Sign On URL** textbox, type hello value using hello following pattern: `https://<instancename>.convercent.com/`</span></span>

     <span data-ttu-id="efff6-164">c.</span><span class="sxs-lookup"><span data-stu-id="efff6-164">c.</span></span> <span data-ttu-id="efff6-165">V hello **předávání stavu** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="efff6-165">In hello **Relay State** textbox, type hello value using hello following pattern: `https://<instancename>.convercent.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="efff6-166">Tyto hodnoty nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="efff6-166">These values are not hello real values.</span></span> <span data-ttu-id="efff6-167">Aktualizovat tyto hodnoty s hello skutečné identifikátor, přihlašovací adresa URL a předávací stavu.</span><span class="sxs-lookup"><span data-stu-id="efff6-167">Update these values with hello actual Identifier, Sign On URL and Relay State.</span></span> <span data-ttu-id="efff6-168">Obraťte se na [tým podpory Convercent klienta](http://support.convercent.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="efff6-168">Contact [Convercent Client support team](http://support.convercent.com) tooget these values.</span></span>

5. <span data-ttu-id="efff6-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="efff6-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_certificate.png) 

6. <span data-ttu-id="efff6-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efff6-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="efff6-173">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory Convercent](mailto:support@convercent.com) a poskytnout hello Stáhnout **soubor XML s metadaty**.</span><span class="sxs-lookup"><span data-stu-id="efff6-173">tooget SSO configured for your application, contact [Convercent support team](mailto:support@convercent.com) and provide them with hello downloaded **Metadata XML**.</span></span>

> [!TIP]
> <span data-ttu-id="efff6-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="efff6-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="efff6-175">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="efff6-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="efff6-176">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="efff6-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="efff6-177">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="efff6-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="efff6-178">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="efff6-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="efff6-180">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="efff6-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="efff6-181">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="efff6-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="efff6-183">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="efff6-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="efff6-185">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="efff6-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="efff6-187">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="efff6-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="efff6-189">a.</span><span class="sxs-lookup"><span data-stu-id="efff6-189">a.</span></span> <span data-ttu-id="efff6-190">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="efff6-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="efff6-191">b.</span><span class="sxs-lookup"><span data-stu-id="efff6-191">b.</span></span> <span data-ttu-id="efff6-192">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="efff6-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="efff6-193">c.</span><span class="sxs-lookup"><span data-stu-id="efff6-193">c.</span></span> <span data-ttu-id="efff6-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="efff6-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="efff6-195">d.</span><span class="sxs-lookup"><span data-stu-id="efff6-195">d.</span></span> <span data-ttu-id="efff6-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="efff6-196">Click **Create**.</span></span>
 
### <a name="creating-a-convercent-test-user"></a><span data-ttu-id="efff6-197">Vytvoření zkušebního uživatele Convercent</span><span class="sxs-lookup"><span data-stu-id="efff6-197">Creating a Convercent test user</span></span>

<span data-ttu-id="efff6-198">Práce s [tým podpory Convercent](mailto:support@convercent.com) tooadd hello uživatelé v platformě Convercent hello.</span><span class="sxs-lookup"><span data-stu-id="efff6-198">Work with [Convercent support team](mailto:support@convercent.com) tooadd hello users in hello Convercent platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="efff6-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="efff6-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="efff6-200">V této části povolíte tak, že udělíte přístup tooConvercent toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="efff6-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooConvercent.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="efff6-202">**tooassign Britta Simon tooConvercent, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="efff6-202">**tooassign Britta Simon tooConvercent, perform hello following steps:**</span></span>

1. <span data-ttu-id="efff6-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="efff6-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="efff6-205">V seznamu aplikace hello vyberte **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="efff6-205">In hello applications list, select **Convercent**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_app.png) 

3. <span data-ttu-id="efff6-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="efff6-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="efff6-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="efff6-209">Click **Add** button.</span></span> <span data-ttu-id="efff6-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="efff6-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="efff6-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="efff6-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="efff6-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="efff6-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="efff6-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="efff6-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="efff6-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="efff6-215">Testing single sign-on</span></span>

<span data-ttu-id="efff6-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="efff6-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="efff6-217">Když kliknete na dlaždici Convercent hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Convercent aplikace.</span><span class="sxs-lookup"><span data-stu-id="efff6-217">When you click hello Convercent tile in hello Access Panel, you should get automatically signed-on tooyour Convercent application.</span></span>
<span data-ttu-id="efff6-218">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="efff6-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="efff6-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="efff6-219">Additional resources</span></span>

* [<span data-ttu-id="efff6-220">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="efff6-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="efff6-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="efff6-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png

