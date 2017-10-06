---
title: 'Kurz: Azure Active Directory integrace s LogicMonitor | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LogicMonitor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: ea5cb8b574d763cb114286e3b2a5c94ab5546756
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="c4b3c-103">Kurz: Azure Active Directory integrace s LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="c4b3c-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="c4b3c-104">V tomto kurzu zjistíte, jak toointegrate LogicMonitor s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c4b3c-104">In this tutorial, you learn how toointegrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c4b3c-105">Integrace LogicMonitor s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-105">Integrating LogicMonitor with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c4b3c-106">Můžete řídit ve službě Azure AD, který má přístup tooLogicMonitor</span><span class="sxs-lookup"><span data-stu-id="c4b3c-106">You can control in Azure AD who has access tooLogicMonitor</span></span>
- <span data-ttu-id="c4b3c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLogicMonitor (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4b3c-107">You can enable your users tooautomatically get signed-on tooLogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c4b3c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c4b3c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c4b3c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c4b3c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4b3c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4b3c-110">Prerequisites</span></span>

<span data-ttu-id="c4b3c-111">Integrace služby Azure AD s LogicMonitor tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-111">tooconfigure Azure AD integration with LogicMonitor, you need hello following items:</span></span>

- <span data-ttu-id="c4b3c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4b3c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c4b3c-113">LogicMonitor jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c4b3c-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c4b3c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c4b3c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c4b3c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c4b3c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c4b3c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c4b3c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c4b3c-118">Scenario description</span></span>
<span data-ttu-id="c4b3c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c4b3c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c4b3c-121">Přidání LogicMonitor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c4b3c-121">Adding LogicMonitor from hello gallery</span></span>
2. <span data-ttu-id="c4b3c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4b3c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-hello-gallery"></a><span data-ttu-id="c4b3c-123">Přidání LogicMonitor z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c4b3c-123">Adding LogicMonitor from hello gallery</span></span>
<span data-ttu-id="c4b3c-124">tooconfigure hello integrace LogicMonitor do Azure AD, je nutné tooadd LogicMonitor hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-124">tooconfigure hello integration of LogicMonitor into Azure AD, you need tooadd LogicMonitor from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c4b3c-125">**tooadd LogicMonitor z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-125">**tooadd LogicMonitor from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4b3c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c4b3c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c4b3c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c4b3c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c4b3c-133">Hello vyhledávacího pole zadejte **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-133">In hello search box, type **LogicMonitor**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="c4b3c-135">Na panelu výsledků hello vyberte **LogicMonitor**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-135">In hello results panel, select **LogicMonitor**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c4b3c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4b3c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c4b3c-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LogicMonitor podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c4b3c-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c4b3c-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LogicMonitor je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LogicMonitor is tooa user in Azure AD.</span></span> <span data-ttu-id="c4b3c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LogicMonitor musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-140">In other words, a link relationship between an Azure AD user and hello related user in LogicMonitor needs toobe established.</span></span>

<span data-ttu-id="c4b3c-141">V LogicMonitor, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-141">In LogicMonitor, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c4b3c-142">tooconfigure a testu Azure AD jednotné přihlašování s LogicMonitor, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-142">tooconfigure and test Azure AD single sign-on with LogicMonitor, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c4b3c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c4b3c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c4b3c-145">**[Vytvoření zkušebního uživatele LogicMonitor](#creating-a-logicmonitor-test-user)**  -toohave protějšek Britta Simon v LogicMonitor, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - toohave a counterpart of Britta Simon in LogicMonitor that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c4b3c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c4b3c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c4b3c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4b3c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c4b3c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="c4b3c-150">**tooconfigure Azure AD jednotné přihlašování s LogicMonitor, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-150">**tooconfigure Azure AD single sign-on with LogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4b3c-151">V portálu Azure, na hello hello **LogicMonitor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-151">In hello Azure portal, on hello **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c4b3c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="c4b3c-155">Na hello **LogicMonitor domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-155">On hello **LogicMonitor Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="c4b3c-157">a.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-157">a.</span></span> <span data-ttu-id="c4b3c-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="c4b3c-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="c4b3c-159">b.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-159">b.</span></span> <span data-ttu-id="c4b3c-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="c4b3c-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c4b3c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-161">These values are not real.</span></span> <span data-ttu-id="c4b3c-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c4b3c-163">Obraťte se na [tým podpory LogicMonitor klienta](https://www.logicmonitor.com/contact/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) tooget these values.</span></span> 
 


4. <span data-ttu-id="c4b3c-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="c4b3c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c4b3c-168">Přihlaste se tooyour **LogicMonitor** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-168">Log in tooyour **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="c4b3c-169">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-169">In hello menu on hello top, click **Settings**.</span></span>
   
   <span data-ttu-id="c4b3c-170">![Nastavení](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="c4b3c-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="c4b3c-171">V navigačním bat hello na levé straně hello, klikněte na tlačítko **jednotné přihlašování**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-171">In hello navigation bat on hello left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="c4b3c-172">![Jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="c4b3c-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="c4b3c-173">V hello **jednotné přihlašování (SSO) nastavení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-173">In hello **Single Sign-on (SSO) settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="c4b3c-174">![Jednotné přihlašování v nastavení](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="c4b3c-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="c4b3c-175">a.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-175">a.</span></span> <span data-ttu-id="c4b3c-176">Vyberte **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="c4b3c-177">b.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-177">b.</span></span> <span data-ttu-id="c4b3c-178">Jako **výchozí přiřazení Role**, vyberte **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="c4b3c-179">c.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-179">c.</span></span> <span data-ttu-id="c4b3c-180">V poznámkovém bloku otevřete hello stáhnout soubor metadat a vložte obsah souboru hello do hello **metadat zprostředkovatelů Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-180">Open hello downloaded metadata file in notepad, and then paste content of hello file into hello **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="c4b3c-181">d.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-181">d.</span></span> <span data-ttu-id="c4b3c-182">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="c4b3c-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c4b3c-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c4b3c-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c4b3c-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c4b3c-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c4b3c-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4b3c-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="c4b3c-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c4b3c-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4b3c-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c4b3c-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c4b3c-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c4b3c-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c4b3c-198">a.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-198">a.</span></span> <span data-ttu-id="c4b3c-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c4b3c-200">b.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-200">b.</span></span> <span data-ttu-id="c4b3c-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c4b3c-202">c.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-202">c.</span></span> <span data-ttu-id="c4b3c-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c4b3c-204">d.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-204">d.</span></span> <span data-ttu-id="c4b3c-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="c4b3c-206">Vytvoření zkušebního uživatele LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="c4b3c-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="c4b3c-207">Pro AAD uživatelé toobe možné toosign v musí být zřízená toohello LogicMonitor aplikace pomocí jejich názvy uživatele Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-207">For AAD users toobe able toosign in, they must be provisioned toohello LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="c4b3c-208">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-208">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4b3c-209">Přihlaste se tooyour LogicMonitor společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-209">Log in tooyour LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="c4b3c-210">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**a potom klikněte na **role a uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-210">In hello menu on hello top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="c4b3c-211">![Role a uživatelé](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "rolí a uživatelů")</span><span class="sxs-lookup"><span data-stu-id="c4b3c-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="c4b3c-212">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-212">Click **Add**.</span></span>

4. <span data-ttu-id="c4b3c-213">V hello **přidat účet** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c4b3c-213">In hello **Add an account** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="c4b3c-214">![Přidat účet](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "přidat účet")</span><span class="sxs-lookup"><span data-stu-id="c4b3c-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="c4b3c-215">a.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-215">a.</span></span> <span data-ttu-id="c4b3c-216">Typ hello **uživatelské jméno**, **e-mailu**, **heslo**, a **znovu zadejte heslo** hodnoty chcete uživatele Azure Active Directory hello tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-216">Type hello **Username**, **Email**, **Password**, and **Retype password** values of hello Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="c4b3c-217">b.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-217">b.</span></span> <span data-ttu-id="c4b3c-218">Vyberte **role**, **zobrazení oprávnění**a hello **stav**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-218">Select **Roles**, **View Permissions**, and hello **Status**.</span></span>
   
   <span data-ttu-id="c4b3c-219">c.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-219">c.</span></span> <span data-ttu-id="c4b3c-220">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="c4b3c-221">Můžete použít všechny ostatní LogicMonitor uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované LogicMonitor tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c4b3c-222">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c4b3c-222">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c4b3c-223">V této části povolíte tak, že udělíte přístup tooLogicMonitor toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-223">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLogicMonitor.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c4b3c-225">**tooassign Britta Simon tooLogicMonitor, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c4b3c-225">**tooassign Britta Simon tooLogicMonitor, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4b3c-226">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-226">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c4b3c-228">V seznamu aplikace hello vyberte **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-228">In hello applications list, select **LogicMonitor**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="c4b3c-230">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-230">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c4b3c-232">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-232">Click **Add** button.</span></span> <span data-ttu-id="c4b3c-233">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c4b3c-235">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-235">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c4b3c-236">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c4b3c-237">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c4b3c-238">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c4b3c-238">Testing single sign-on</span></span>

<span data-ttu-id="c4b3c-239">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-239">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="c4b3c-240">Když kliknete na dlaždici LogicMonitor hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LogicMonitor aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4b3c-240">When you click hello LogicMonitor tile in hello Access Panel, you should get automatically signed-on tooyour LogicMonitor application.</span></span>
<span data-ttu-id="c4b3c-241">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c4b3c-241">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c4b3c-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c4b3c-242">Additional resources</span></span>

* [<span data-ttu-id="c4b3c-243">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4b3c-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c4b3c-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c4b3c-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

