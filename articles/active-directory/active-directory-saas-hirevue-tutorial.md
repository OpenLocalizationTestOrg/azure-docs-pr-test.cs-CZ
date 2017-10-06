---
title: 'Kurz: Azure Active Directory integrace s HireVue | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a HireVue."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aadfc342-14db-4d74-a83d-f0c76f0cf63c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f890633ee4f13919c32a43d7b6cf30f2270ef6a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a><span data-ttu-id="04e17-103">Kurz: Azure Active Directory integrace s HireVue</span><span class="sxs-lookup"><span data-stu-id="04e17-103">Tutorial: Azure Active Directory integration with HireVue</span></span>

<span data-ttu-id="04e17-104">V tomto kurzu zjistíte, jak toointegrate HireVue s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="04e17-104">In this tutorial, you learn how toointegrate HireVue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="04e17-105">Integrace HireVue s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="04e17-105">Integrating HireVue with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="04e17-106">Můžete řídit ve službě Azure AD, který má přístup tooHireVue</span><span class="sxs-lookup"><span data-stu-id="04e17-106">You can control in Azure AD who has access tooHireVue</span></span>
- <span data-ttu-id="04e17-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHireVue (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="04e17-107">You can enable your users tooautomatically get signed-on tooHireVue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="04e17-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="04e17-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="04e17-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="04e17-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04e17-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="04e17-110">Prerequisites</span></span>

<span data-ttu-id="04e17-111">Integrace služby Azure AD s HireVue tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="04e17-111">tooconfigure Azure AD integration with HireVue, you need hello following items:</span></span>

- <span data-ttu-id="04e17-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="04e17-112">An Azure AD subscription</span></span>
- <span data-ttu-id="04e17-113">HireVue jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="04e17-113">A HireVue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="04e17-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="04e17-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="04e17-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="04e17-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="04e17-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="04e17-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="04e17-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="04e17-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="04e17-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="04e17-118">Scenario description</span></span>
<span data-ttu-id="04e17-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="04e17-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="04e17-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="04e17-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="04e17-121">Přidání HireVue z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="04e17-121">Adding HireVue from hello gallery</span></span>
2. <span data-ttu-id="04e17-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04e17-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hirevue-from-hello-gallery"></a><span data-ttu-id="04e17-123">Přidání HireVue z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="04e17-123">Adding HireVue from hello gallery</span></span>
<span data-ttu-id="04e17-124">tooconfigure hello integrace HireVue do Azure AD, je nutné tooadd HireVue hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="04e17-124">tooconfigure hello integration of HireVue into Azure AD, you need tooadd HireVue from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="04e17-125">**tooadd HireVue z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04e17-125">**tooadd HireVue from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="04e17-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04e17-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="04e17-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="04e17-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="04e17-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04e17-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="04e17-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04e17-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="04e17-133">Hello vyhledávacího pole zadejte **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="04e17-133">In hello search box, type **HireVue**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_search.png)

5. <span data-ttu-id="04e17-135">Na panelu výsledků hello vyberte **HireVue**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e17-135">In hello results panel, select **HireVue**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="04e17-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="04e17-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="04e17-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s HireVue podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="04e17-138">In this section, you configure and test Azure AD single sign-on with HireVue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="04e17-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v HireVue je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="04e17-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HireVue is tooa user in Azure AD.</span></span> <span data-ttu-id="04e17-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v HireVue musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="04e17-140">In other words, a link relationship between an Azure AD user and hello related user in HireVue needs toobe established.</span></span>

<span data-ttu-id="04e17-141">V HireVue, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="04e17-141">In HireVue, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="04e17-142">tooconfigure a testu Azure AD jednotné přihlašování s HireVue, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="04e17-142">tooconfigure and test Azure AD single sign-on with HireVue, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="04e17-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="04e17-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="04e17-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="04e17-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="04e17-145">**[Vytvoření zkušebního uživatele HireVue](#creating-a-hirevue-test-user)**  -toohave protějšek Britta Simon v HireVue, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="04e17-145">**[Creating a HireVue test user](#creating-a-hirevue-test-user)** - toohave a counterpart of Britta Simon in HireVue that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="04e17-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04e17-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="04e17-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="04e17-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="04e17-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04e17-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="04e17-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci HireVue.</span><span class="sxs-lookup"><span data-stu-id="04e17-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HireVue application.</span></span>

<span data-ttu-id="04e17-150">**tooconfigure Azure AD jednotné přihlašování s HireVue, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04e17-150">**tooconfigure Azure AD single sign-on with HireVue, perform hello following steps:**</span></span>

1. <span data-ttu-id="04e17-151">V portálu Azure, na hello hello **HireVue** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="04e17-151">In hello Azure portal, on hello **HireVue** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="04e17-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04e17-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_samlbase.png)

3. <span data-ttu-id="04e17-155">Na hello **HireVue domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="04e17-155">On hello **HireVue Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_url.png)

    <span data-ttu-id="04e17-157">a.</span><span class="sxs-lookup"><span data-stu-id="04e17-157">a.</span></span> <span data-ttu-id="04e17-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="04e17-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>

    | <span data-ttu-id="04e17-159">Prostředí</span><span class="sxs-lookup"><span data-stu-id="04e17-159">Environment</span></span> | <span data-ttu-id="04e17-160">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="04e17-160">URL</span></span> |
    |-------------|---|
    | <span data-ttu-id="04e17-161">Produkční</span><span class="sxs-lookup"><span data-stu-id="04e17-161">Production</span></span> | `https://<companyname>.hirevue.com` |
    | <span data-ttu-id="04e17-162">Pracovní</span><span class="sxs-lookup"><span data-stu-id="04e17-162">Staging</span></span>    | `https://<companyname>.stghv.com` |
    
    <span data-ttu-id="04e17-163">b.</span><span class="sxs-lookup"><span data-stu-id="04e17-163">b.</span></span> <span data-ttu-id="04e17-164">V hello **identifikátor** textovému poli, zadejte adresu URL jako:</span><span class="sxs-lookup"><span data-stu-id="04e17-164">In hello **Identifier** textbox, type a URL as:</span></span>
    
    | <span data-ttu-id="04e17-165">Prostředí</span><span class="sxs-lookup"><span data-stu-id="04e17-165">Environment</span></span> | <span data-ttu-id="04e17-166">NÁZEV URN</span><span class="sxs-lookup"><span data-stu-id="04e17-166">URN</span></span> |
    |-------------|-----|
    | <span data-ttu-id="04e17-167">Produkční</span><span class="sxs-lookup"><span data-stu-id="04e17-167">Production</span></span> |`urn:federation:hirevue.com:saml:sp:prod` |
    | <span data-ttu-id="04e17-168">Pracovní</span><span class="sxs-lookup"><span data-stu-id="04e17-168">Staging</span></span>    | `urn:federation:hirevue.com:saml:sp:staging`|
    
    > [!NOTE] 
    > <span data-ttu-id="04e17-169">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="04e17-169">These values are not real.</span></span> <span data-ttu-id="04e17-170">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="04e17-170">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="04e17-171">Obraťte se na [tým podpory HireVue klienta](mailto:samlsupport@hirevue.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="04e17-171">Contact [HireVue Client support team](mailto:samlsupport@hirevue.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="04e17-172">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="04e17-172">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_certificate.png) 

5. <span data-ttu-id="04e17-174">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04e17-174">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="04e17-176">Na hello **HireVue konfigurace** klikněte na tlačítko **konfigurace HireVue** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="04e17-176">On hello **HireVue Configuration** section, click **Configure HireVue** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="04e17-177">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="04e17-177">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_configure.png) 

7. <span data-ttu-id="04e17-179">tooconfigure jednotného přihlašování na **HireVue** straně, je nutné stáhnout hello toosend **Certificate(Base64)** a **Sign-Out URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory HireVue](mailto:samlsupport@hirevue.com).</span><span class="sxs-lookup"><span data-stu-id="04e17-179">tooconfigure single sign-on on **HireVue** side, you need toosend hello downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[HireVue support team](mailto:samlsupport@hirevue.com).</span></span> <span data-ttu-id="04e17-180">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="04e17-180">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="04e17-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="04e17-181">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="04e17-182">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="04e17-182">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="04e17-183">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="04e17-183">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="04e17-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="04e17-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="04e17-185">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="04e17-185">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="04e17-187">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04e17-187">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="04e17-188">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="04e17-188">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="04e17-190">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="04e17-190">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="04e17-192">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="04e17-192">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="04e17-194">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="04e17-194">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="04e17-196">a.</span><span class="sxs-lookup"><span data-stu-id="04e17-196">a.</span></span> <span data-ttu-id="04e17-197">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="04e17-197">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="04e17-198">b.</span><span class="sxs-lookup"><span data-stu-id="04e17-198">b.</span></span> <span data-ttu-id="04e17-199">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="04e17-199">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="04e17-200">c.</span><span class="sxs-lookup"><span data-stu-id="04e17-200">c.</span></span> <span data-ttu-id="04e17-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="04e17-201">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="04e17-202">d.</span><span class="sxs-lookup"><span data-stu-id="04e17-202">d.</span></span> <span data-ttu-id="04e17-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="04e17-203">Click **Create**.</span></span>
 
### <a name="creating-a-hirevue-test-user"></a><span data-ttu-id="04e17-204">Vytvoření zkušebního uživatele HireVue</span><span class="sxs-lookup"><span data-stu-id="04e17-204">Creating a HireVue test user</span></span>

<span data-ttu-id="04e17-205">V této části vytvoříte volal Britta Simon v HireVue uživatele.</span><span class="sxs-lookup"><span data-stu-id="04e17-205">In this section, you create a user called Britta Simon in HireVue.</span></span> <span data-ttu-id="04e17-206">Spojte se s [tým podpory HireVue](mailto:samlsupport@hirevue.com) tooadd hello uživatelé v platformě HireVue hello.</span><span class="sxs-lookup"><span data-stu-id="04e17-206">Please work with [HireVue support team](mailto:samlsupport@hirevue.com) tooadd hello users in hello HireVue platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="04e17-207">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="04e17-207">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="04e17-208">V této části povolíte tak, že udělíte přístup tooHireVue toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="04e17-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHireVue.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="04e17-210">**tooassign Britta Simon tooHireVue, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="04e17-210">**tooassign Britta Simon tooHireVue, perform hello following steps:**</span></span>

1. <span data-ttu-id="04e17-211">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04e17-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="04e17-213">V seznamu aplikace hello vyberte **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="04e17-213">In hello applications list, select **HireVue**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_app.png) 

3. <span data-ttu-id="04e17-215">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="04e17-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="04e17-217">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="04e17-217">Click **Add** button.</span></span> <span data-ttu-id="04e17-218">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04e17-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="04e17-220">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="04e17-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="04e17-221">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04e17-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="04e17-222">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="04e17-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="04e17-223">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="04e17-223">Testing single sign-on</span></span>

<span data-ttu-id="04e17-224">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="04e17-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="04e17-225">Když kliknete na dlaždici HireVue hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour HireVue aplikace.</span><span class="sxs-lookup"><span data-stu-id="04e17-225">When you click hello HireVue tile in hello Access Panel, you should get automatically signed-on tooyour HireVue application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04e17-226">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="04e17-226">Additional resources</span></span>

* [<span data-ttu-id="04e17-227">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="04e17-227">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="04e17-228">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="04e17-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png

