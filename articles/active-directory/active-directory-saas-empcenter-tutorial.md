---
title: 'Kurz: Azure Active Directory integrace s EmpCenter | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a EmpCenter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: cce5d86db550327eb73660fb78cea7e6d5428890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a><span data-ttu-id="709e0-103">Kurz: Azure Active Directory integrace s EmpCenter</span><span class="sxs-lookup"><span data-stu-id="709e0-103">Tutorial: Azure Active Directory integration with EmpCenter</span></span>

<span data-ttu-id="709e0-104">V tomto kurzu zjistíte, jak toointegrate EmpCenter s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="709e0-104">In this tutorial, you learn how toointegrate EmpCenter with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="709e0-105">Integrace EmpCenter s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="709e0-105">Integrating EmpCenter with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="709e0-106">Můžete řídit ve službě Azure AD, který má přístup tooEmpCenter</span><span class="sxs-lookup"><span data-stu-id="709e0-106">You can control in Azure AD who has access tooEmpCenter</span></span>
- <span data-ttu-id="709e0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooEmpCenter (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="709e0-107">You can enable your users tooautomatically get signed-on tooEmpCenter (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="709e0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="709e0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="709e0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="709e0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="709e0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="709e0-110">Prerequisites</span></span>

<span data-ttu-id="709e0-111">Integrace služby Azure AD s EmpCenter tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="709e0-111">tooconfigure Azure AD integration with EmpCenter, you need hello following items:</span></span>

- <span data-ttu-id="709e0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="709e0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="709e0-113">EmpCenter jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="709e0-113">An EmpCenter single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="709e0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="709e0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="709e0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="709e0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="709e0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="709e0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="709e0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="709e0-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="709e0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="709e0-118">Scenario description</span></span>
<span data-ttu-id="709e0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="709e0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="709e0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="709e0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="709e0-121">Přidání EmpCenter z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="709e0-121">Adding EmpCenter from hello gallery</span></span>
2. <span data-ttu-id="709e0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="709e0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-empcenter-from-hello-gallery"></a><span data-ttu-id="709e0-123">Přidání EmpCenter z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="709e0-123">Adding EmpCenter from hello gallery</span></span>
<span data-ttu-id="709e0-124">tooconfigure hello integrace EmpCenter do Azure AD, je nutné tooadd EmpCenter hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="709e0-124">tooconfigure hello integration of EmpCenter into Azure AD, you need tooadd EmpCenter from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="709e0-125">**tooadd EmpCenter z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="709e0-125">**tooadd EmpCenter from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="709e0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="709e0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="709e0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="709e0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="709e0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="709e0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="709e0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="709e0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="709e0-133">Hello vyhledávacího pole zadejte **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="709e0-133">In hello search box, type **EmpCenter**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_search.png)

5. <span data-ttu-id="709e0-135">Na panelu výsledků hello vyberte **EmpCenter**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="709e0-135">In hello results panel, select **EmpCenter**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="709e0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="709e0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="709e0-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s EmpCenter podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="709e0-138">In this section, you configure and test Azure AD single sign-on with EmpCenter based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="709e0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v EmpCenter je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="709e0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EmpCenter is tooa user in Azure AD.</span></span> <span data-ttu-id="709e0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v EmpCenter musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="709e0-140">In other words, a link relationship between an Azure AD user and hello related user in EmpCenter needs toobe established.</span></span>

<span data-ttu-id="709e0-141">V EmpCenter, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="709e0-141">In EmpCenter, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="709e0-142">tooconfigure a testu Azure AD jednotné přihlašování s EmpCenter, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="709e0-142">tooconfigure and test Azure AD single sign-on with EmpCenter, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="709e0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="709e0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="709e0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="709e0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="709e0-145">**[Vytváření testovacího uživatele EmpCenter](#creating-an-empcenter-test-user)**  -toohave protějšek Britta Simon v EmpCenter, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="709e0-145">**[Creating an EmpCenter test user](#creating-an-empcenter-test-user)** - toohave a counterpart of Britta Simon in EmpCenter that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="709e0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="709e0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="709e0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="709e0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="709e0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="709e0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="709e0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci EmpCenter.</span><span class="sxs-lookup"><span data-stu-id="709e0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EmpCenter application.</span></span>

<span data-ttu-id="709e0-150">**tooconfigure Azure AD jednotné přihlašování s EmpCenter, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="709e0-150">**tooconfigure Azure AD single sign-on with EmpCenter, perform hello following steps:**</span></span>

1. <span data-ttu-id="709e0-151">V portálu Azure, na hello hello **EmpCenter** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="709e0-151">In hello Azure portal, on hello **EmpCenter** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="709e0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="709e0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_samlbase.png)

3. <span data-ttu-id="709e0-155">Na hello **EmpCenter domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="709e0-155">On hello **EmpCenter Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_url.png)

    <span data-ttu-id="709e0-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="709e0-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.EmpCenter.com/<instancename>` |
    | `https://<subdomain>.workforcehosting.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="709e0-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="709e0-158">hello value is not real.</span></span> <span data-ttu-id="709e0-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="709e0-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="709e0-160">Obraťte se na [tým podpory EmpCenter klienta](http://www.workforcesoftware.com/services/customer-support/) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="709e0-160">Contact [EmpCenter Client support team](http://www.workforcesoftware.com/services/customer-support/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="709e0-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="709e0-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_certificate.png) 

5. <span data-ttu-id="709e0-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="709e0-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="709e0-165">tooconfigure jednotného přihlašování na **EmpCenter** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory EmpCenter](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="709e0-165">tooconfigure single sign-on on **EmpCenter** side, you need toosend hello downloaded **Metadata XML** too[EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span> <span data-ttu-id="709e0-166">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="709e0-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="709e0-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="709e0-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="709e0-168">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="709e0-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="709e0-169">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="709e0-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="709e0-170">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="709e0-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="709e0-171">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="709e0-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="709e0-173">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="709e0-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="709e0-174">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="709e0-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="709e0-176">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="709e0-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="709e0-178">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="709e0-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="709e0-180">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="709e0-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="709e0-182">a.</span><span class="sxs-lookup"><span data-stu-id="709e0-182">a.</span></span> <span data-ttu-id="709e0-183">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="709e0-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="709e0-184">b.</span><span class="sxs-lookup"><span data-stu-id="709e0-184">b.</span></span> <span data-ttu-id="709e0-185">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="709e0-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="709e0-186">c.</span><span class="sxs-lookup"><span data-stu-id="709e0-186">c.</span></span> <span data-ttu-id="709e0-187">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="709e0-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="709e0-188">d.</span><span class="sxs-lookup"><span data-stu-id="709e0-188">d.</span></span> <span data-ttu-id="709e0-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="709e0-189">Click **Create**.</span></span>
 
### <a name="creating-an-empcenter-test-user"></a><span data-ttu-id="709e0-190">Vytváření testovacího uživatele EmpCenter</span><span class="sxs-lookup"><span data-stu-id="709e0-190">Creating an EmpCenter test user</span></span>

<span data-ttu-id="709e0-191">V pořadí tooenable Azure AD Uživatelé toolog v tooEmpCenter musí být zřízená do EmpCenter.</span><span class="sxs-lookup"><span data-stu-id="709e0-191">In order tooenable Azure AD users toolog in tooEmpCenter, they must be provisioned into EmpCenter.</span></span> <span data-ttu-id="709e0-192">V případě hello EmpCenter, hello uživatelské účty musí toobe vytvořené vaše [tým podpory EmpCenter](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="709e0-192">In hello case of EmpCenter, hello user accounts need toobe created by your [EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span>

> [!NOTE]
> <span data-ttu-id="709e0-193">Můžete použít všechny ostatní EmpCenter uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované EmpCenter tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="709e0-193">You can use any other EmpCenter user account creation tools or APIs provided by EmpCenter tooprovision Azure Active Directory user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="709e0-194">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="709e0-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="709e0-195">V této části povolíte tak, že udělíte přístup tooEmpCenter toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="709e0-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEmpCenter.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="709e0-197">**tooassign Britta Simon tooEmpCenter, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="709e0-197">**tooassign Britta Simon tooEmpCenter, perform hello following steps:**</span></span>

1. <span data-ttu-id="709e0-198">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="709e0-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="709e0-200">V seznamu aplikace hello vyberte **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="709e0-200">In hello applications list, select **EmpCenter**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_app.png) 

3. <span data-ttu-id="709e0-202">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="709e0-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="709e0-204">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="709e0-204">Click **Add** button.</span></span> <span data-ttu-id="709e0-205">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="709e0-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="709e0-207">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="709e0-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="709e0-208">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="709e0-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="709e0-209">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="709e0-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="709e0-210">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="709e0-210">Testing single sign-on</span></span>


<span data-ttu-id="709e0-211">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="709e0-211">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="709e0-212">Když kliknete na dlaždici EmpCenter hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour EmpCenter aplikace.</span><span class="sxs-lookup"><span data-stu-id="709e0-212">When you click hello EmpCenter tile in hello Access Panel, you should get automatically signed-on tooyour EmpCenter application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="709e0-213">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="709e0-213">Additional resources</span></span>

* [<span data-ttu-id="709e0-214">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="709e0-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="709e0-215">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="709e0-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_203.png

