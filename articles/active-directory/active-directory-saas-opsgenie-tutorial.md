---
title: 'Kurz: Azure Active Directory integrace s OpsGenie | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a OpsGenie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 50d31c234fb9716d05d681b9bc4164740a3a662b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="504ac-103">Kurz: Azure Active Directory integrace s OpsGenie</span><span class="sxs-lookup"><span data-stu-id="504ac-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="504ac-104">V tomto kurzu zjistíte, jak toointegrate OpsGenie s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="504ac-104">In this tutorial, you learn how toointegrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="504ac-105">Integrace OpsGenie s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="504ac-105">Integrating OpsGenie with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="504ac-106">Můžete řídit ve službě Azure AD, který má přístup tooOpsGenie</span><span class="sxs-lookup"><span data-stu-id="504ac-106">You can control in Azure AD who has access tooOpsGenie</span></span>
- <span data-ttu-id="504ac-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooOpsGenie (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="504ac-107">You can enable your users tooautomatically get signed-on tooOpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="504ac-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="504ac-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="504ac-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="504ac-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="504ac-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="504ac-110">Prerequisites</span></span>

<span data-ttu-id="504ac-111">Integrace služby Azure AD s OpsGenie tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="504ac-111">tooconfigure Azure AD integration with OpsGenie, you need hello following items:</span></span>

- <span data-ttu-id="504ac-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="504ac-112">An Azure AD subscription</span></span>
- <span data-ttu-id="504ac-113">OpsGenie jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="504ac-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="504ac-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="504ac-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="504ac-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="504ac-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="504ac-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="504ac-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="504ac-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="504ac-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="504ac-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="504ac-118">Scenario description</span></span>
<span data-ttu-id="504ac-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="504ac-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="504ac-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="504ac-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="504ac-121">Přidání OpsGenie z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="504ac-121">Adding OpsGenie from hello gallery</span></span>
2. <span data-ttu-id="504ac-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="504ac-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-hello-gallery"></a><span data-ttu-id="504ac-123">Přidání OpsGenie z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="504ac-123">Adding OpsGenie from hello gallery</span></span>
<span data-ttu-id="504ac-124">tooconfigure hello integrace OpsGenie do Azure AD, je nutné tooadd OpsGenie hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="504ac-124">tooconfigure hello integration of OpsGenie into Azure AD, you need tooadd OpsGenie from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="504ac-125">**tooadd OpsGenie z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="504ac-125">**tooadd OpsGenie from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="504ac-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="504ac-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="504ac-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="504ac-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="504ac-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="504ac-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="504ac-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="504ac-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="504ac-133">Hello vyhledávacího pole zadejte **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="504ac-133">In hello search box, type **OpsGenie**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="504ac-135">Na panelu výsledků hello vyberte **OpsGenie**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="504ac-135">In hello results panel, select **OpsGenie**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="504ac-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="504ac-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="504ac-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s OpsGenie podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="504ac-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="504ac-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v OpsGenie je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="504ac-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OpsGenie is tooa user in Azure AD.</span></span> <span data-ttu-id="504ac-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v OpsGenie musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="504ac-140">In other words, a link relationship between an Azure AD user and hello related user in OpsGenie needs toobe established.</span></span>

<span data-ttu-id="504ac-141">V OpsGenie, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="504ac-141">In OpsGenie, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="504ac-142">tooconfigure a testu Azure AD jednotné přihlašování s OpsGenie, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="504ac-142">tooconfigure and test Azure AD single sign-on with OpsGenie, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="504ac-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="504ac-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="504ac-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="504ac-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="504ac-145">**[Vytvoření zkušebního uživatele OpsGenie](#creating-a-opsgenie-test-user)**  -toohave protějšek Britta Simon v OpsGenie, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="504ac-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - toohave a counterpart of Britta Simon in OpsGenie that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="504ac-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="504ac-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="504ac-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="504ac-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="504ac-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="504ac-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="504ac-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="504ac-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="504ac-150">**tooconfigure Azure AD jednotné přihlašování s OpsGenie, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="504ac-150">**tooconfigure Azure AD single sign-on with OpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="504ac-151">V portálu Azure, na hello hello **OpsGenie** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="504ac-151">In hello Azure portal, on hello **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="504ac-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="504ac-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="504ac-155">Na hello **OpsGenie domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="504ac-155">On hello **OpsGenie Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="504ac-157">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="504ac-157">In hello **Sign-on URL** textbox, type hello URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="504ac-158">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="504ac-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="504ac-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="504ac-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="504ac-162">Na hello **OpsGenie konfigurace** klikněte na tlačítko **konfigurace OpsGenie** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="504ac-162">On hello **OpsGenie Configuration** section, click **Configure OpsGenie** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="504ac-163">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="504ac-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="504ac-165">Otevřete jinou instanci prohlížeče a pak přihlášení tooOpsGenie jako správce.</span><span class="sxs-lookup"><span data-stu-id="504ac-165">Open another browser instance, and then log-in tooOpsGenie as an administrator.</span></span>

8. <span data-ttu-id="504ac-166">Klikněte na tlačítko **nastavení**a potom klikněte na hello **jednotné přihlašování** kartě.</span><span class="sxs-lookup"><span data-stu-id="504ac-166">Click **Settings**, and then click hello **Single Sign On** tab.</span></span>
   
    ![OpsGenie jednotné přihlášení](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="504ac-168">Vyberte tooenable jednotné přihlašování, **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="504ac-168">tooenable SSO, select **Enabled**.</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="504ac-170">V hello **zprostředkovatele** klikněte na tlačítko hello **Azure Active Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="504ac-170">In hello **Provider** section, click hello **Azure Active Directory** tab.</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="504ac-172">Na stránce dialogové okno hello Azure Active Directory proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="504ac-172">On hello Azure Active Directory dialog page, perform hello following steps:</span></span>
   
    ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="504ac-174">a.</span><span class="sxs-lookup"><span data-stu-id="504ac-174">a.</span></span> <span data-ttu-id="504ac-175">Vložení **jeden znak na adresu URL služby**, který jste zkopírovali z hello portálu Azure do hello **SAML 2.0 Endpoint** textové pole.</span><span class="sxs-lookup"><span data-stu-id="504ac-175">Paste **Single Sign On Service URL**, which you have copied from hello Azure portal into hello **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="504ac-176">b.</span><span class="sxs-lookup"><span data-stu-id="504ac-176">b.</span></span> <span data-ttu-id="504ac-177">Stažený kódovaného certifikátu kódování base-64 otevřete v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit do hello **X.500 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="504ac-177">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="504ac-178">c.</span><span class="sxs-lookup"><span data-stu-id="504ac-178">c.</span></span> <span data-ttu-id="504ac-179">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="504ac-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="504ac-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="504ac-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="504ac-181">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="504ac-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="504ac-182">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="504ac-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="504ac-183">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="504ac-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="504ac-184">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="504ac-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="504ac-186">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="504ac-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="504ac-187">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="504ac-187">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="504ac-189">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="504ac-189">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="504ac-191">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="504ac-191">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="504ac-193">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="504ac-193">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="504ac-195">a.</span><span class="sxs-lookup"><span data-stu-id="504ac-195">a.</span></span> <span data-ttu-id="504ac-196">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="504ac-196">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="504ac-197">b.</span><span class="sxs-lookup"><span data-stu-id="504ac-197">b.</span></span> <span data-ttu-id="504ac-198">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="504ac-198">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="504ac-199">c.</span><span class="sxs-lookup"><span data-stu-id="504ac-199">c.</span></span> <span data-ttu-id="504ac-200">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="504ac-200">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="504ac-201">d.</span><span class="sxs-lookup"><span data-stu-id="504ac-201">d.</span></span> <span data-ttu-id="504ac-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="504ac-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="504ac-203">Vytvoření zkušebního uživatele OpsGenie</span><span class="sxs-lookup"><span data-stu-id="504ac-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="504ac-204">Hello cílem této části je toocreate volal Britta Simon v OpsGenie uživatele.</span><span class="sxs-lookup"><span data-stu-id="504ac-204">hello objective of this section is toocreate a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="504ac-205">V okně webového prohlížeče Přihlaste se ke klientovi OpsGenie jako správce.</span><span class="sxs-lookup"><span data-stu-id="504ac-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="504ac-206">Přejděte tooUsers seznamu kliknutím **uživatele** v levém panelu.</span><span class="sxs-lookup"><span data-stu-id="504ac-206">Navigate tooUsers list by clicking **User** in left panel.</span></span>
   
   ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="504ac-208">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="504ac-208">Click **Add User**.</span></span>

4. <span data-ttu-id="504ac-209">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="504ac-209">On hello **Add User** dialog, perform hello following steps:</span></span>
   
   ![Nastavení OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="504ac-211">a.</span><span class="sxs-lookup"><span data-stu-id="504ac-211">a.</span></span> <span data-ttu-id="504ac-212">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu BrittaSimon řešit v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="504ac-212">In hello **Email** textbox, type hello email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="504ac-213">b.</span><span class="sxs-lookup"><span data-stu-id="504ac-213">b.</span></span> <span data-ttu-id="504ac-214">V hello **úplný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="504ac-214">In hello **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="504ac-215">c.</span><span class="sxs-lookup"><span data-stu-id="504ac-215">c.</span></span> <span data-ttu-id="504ac-216">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="504ac-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="504ac-217">Britta získá e-mail s pokyny k nastavení svůj profil.</span><span class="sxs-lookup"><span data-stu-id="504ac-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="504ac-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="504ac-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="504ac-219">V této části povolíte tak, že udělíte přístup tooOpsGenie toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="504ac-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOpsGenie.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="504ac-221">**tooassign Britta Simon tooOpsGenie, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="504ac-221">**tooassign Britta Simon tooOpsGenie, perform hello following steps:**</span></span>

1. <span data-ttu-id="504ac-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="504ac-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="504ac-224">V seznamu aplikace hello vyberte **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="504ac-224">In hello applications list, select **OpsGenie**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="504ac-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="504ac-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="504ac-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="504ac-228">Click **Add** button.</span></span> <span data-ttu-id="504ac-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="504ac-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="504ac-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="504ac-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="504ac-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="504ac-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="504ac-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="504ac-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="504ac-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="504ac-234">Testing single sign-on</span></span>

<span data-ttu-id="504ac-235">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="504ac-235">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="504ac-236">Když kliknete na dlaždici OpsGenie hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour OpsGenie aplikace.</span><span class="sxs-lookup"><span data-stu-id="504ac-236">When you click hello OpsGenie tile in hello Access Panel, you should get automatically signed-on tooyour OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="504ac-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="504ac-237">Additional resources</span></span>

* [<span data-ttu-id="504ac-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="504ac-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="504ac-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="504ac-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

