---
title: 'Kurz: Azure Active Directory integrace s Jobscience | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jobscience."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a><span data-ttu-id="b969f-103">Kurz: Azure Active Directory integrace s Jobscience</span><span class="sxs-lookup"><span data-stu-id="b969f-103">Tutorial: Azure Active Directory integration with Jobscience</span></span>

<span data-ttu-id="b969f-104">V tomto kurzu zjistíte, jak toointegrate Jobscience s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b969f-104">In this tutorial, you learn how toointegrate Jobscience with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b969f-105">Integrace Jobscience s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b969f-105">Integrating Jobscience with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b969f-106">Můžete řídit ve službě Azure AD, který má přístup tooJobscience</span><span class="sxs-lookup"><span data-stu-id="b969f-106">You can control in Azure AD who has access tooJobscience</span></span>
- <span data-ttu-id="b969f-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooJobscience (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b969f-107">You can enable your users tooautomatically get signed-on tooJobscience (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b969f-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b969f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b969f-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b969f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b969f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b969f-110">Prerequisites</span></span>

<span data-ttu-id="b969f-111">Integrace služby Azure AD s Jobscience tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b969f-111">tooconfigure Azure AD integration with Jobscience, you need hello following items:</span></span>

- <span data-ttu-id="b969f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b969f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b969f-113">Jobscience jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b969f-113">A Jobscience single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b969f-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b969f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b969f-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b969f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b969f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b969f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b969f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b969f-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b969f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b969f-118">Scenario description</span></span>
<span data-ttu-id="b969f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b969f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b969f-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b969f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b969f-121">Přidání Jobscience z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b969f-121">Adding Jobscience from hello gallery</span></span>
2. <span data-ttu-id="b969f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b969f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscience-from-hello-gallery"></a><span data-ttu-id="b969f-123">Přidání Jobscience z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b969f-123">Adding Jobscience from hello gallery</span></span>
<span data-ttu-id="b969f-124">tooconfigure hello integrace Jobscience do Azure AD, je nutné tooadd Jobscience hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b969f-124">tooconfigure hello integration of Jobscience into Azure AD, you need tooadd Jobscience from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b969f-125">**tooadd Jobscience z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b969f-125">**tooadd Jobscience from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b969f-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b969f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b969f-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b969f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b969f-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b969f-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b969f-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b969f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b969f-133">Hello vyhledávacího pole zadejte **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="b969f-133">In hello search box, type **Jobscience**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. <span data-ttu-id="b969f-135">Na panelu výsledků hello vyberte **Jobscience**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b969f-135">In hello results panel, select **Jobscience**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b969f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b969f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b969f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobscience podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="b969f-138">In this section, you configure and test Azure AD single sign-on with Jobscience based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b969f-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Jobscience je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b969f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jobscience is tooa user in Azure AD.</span></span> <span data-ttu-id="b969f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Jobscience musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b969f-140">In other words, a link relationship between an Azure AD user and hello related user in Jobscience needs toobe established.</span></span>

<span data-ttu-id="b969f-141">V Jobscience, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b969f-141">In Jobscience, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b969f-142">tooconfigure a testu Azure AD jednotné přihlašování s Jobscience, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b969f-142">tooconfigure and test Azure AD single sign-on with Jobscience, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b969f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b969f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b969f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b969f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b969f-145">**[Vytvoření zkušebního uživatele Jobscience](#creating-a-jobscience-test-user)**  -toohave protějšek Britta Simon v Jobscience, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b969f-145">**[Creating a Jobscience test user](#creating-a-jobscience-test-user)** - toohave a counterpart of Britta Simon in Jobscience that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b969f-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b969f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b969f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b969f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b969f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b969f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b969f-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Jobscience.</span><span class="sxs-lookup"><span data-stu-id="b969f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jobscience application.</span></span>

<span data-ttu-id="b969f-150">**tooconfigure Azure AD jednotné přihlašování s Jobscience, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b969f-150">**tooconfigure Azure AD single sign-on with Jobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="b969f-151">V portálu Azure, na hello hello **Jobscience** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b969f-151">In hello Azure portal, on hello **Jobscience** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b969f-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b969f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. <span data-ttu-id="b969f-155">Na hello **Jobscience domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b969f-155">On hello **Jobscience Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    <span data-ttu-id="b969f-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://<company name>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="b969f-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `http://<company name>.my.salesforce.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="b969f-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b969f-158">This value is not real.</span></span> <span data-ttu-id="b969f-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b969f-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b969f-160">Tato hodnota získat [tým podpory Jobscience klienta](https://www.jobscience.com/support) nebo z hello jednotného přihlašování k profilu bude vytvořena, což je vysvětleno později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="b969f-160">Get this value by [Jobscience Client support team](https://www.jobscience.com/support) or from hello SSO profile you will create which is explained later in hello tutorial.</span></span> 
 
4. <span data-ttu-id="b969f-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b969f-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. <span data-ttu-id="b969f-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b969f-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b969f-165">Na hello **Jobscience konfigurace** klikněte na tlačítko **konfigurace Jobscience** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b969f-165">On hello **Jobscience Configuration** section, click **Configure Jobscience** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b969f-166">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b969f-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. <span data-ttu-id="b969f-168">Přihlaste se tooyour Jobscience společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b969f-168">Log in tooyour Jobscience company site as an administrator.</span></span>

8. <span data-ttu-id="b969f-169">Přejděte příliš**instalační program**.</span><span class="sxs-lookup"><span data-stu-id="b969f-169">Go too**Setup**.</span></span>
   
   <span data-ttu-id="b969f-170">![Instalační program](./media/active-directory-saas-jobscience-tutorial/IC784358.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="b969f-170">![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")</span></span>

9. <span data-ttu-id="b969f-171">V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky.</span><span class="sxs-lookup"><span data-stu-id="b969f-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
   <span data-ttu-id="b969f-172">![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="b969f-172">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="b969f-173">tooverify, který vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazené tooUsers**" a zkontrolovat vaše "**Moje nastavení domény**".</span><span class="sxs-lookup"><span data-stu-id="b969f-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>

    <span data-ttu-id="b969f-174">![Domény nasazen tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "tooUser nasazené v doméně")</span><span class="sxs-lookup"><span data-stu-id="b969f-174">![Domain Deployed tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="b969f-175">Na webu společnosti Jobscience hello, klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b969f-175">On hello Jobscience company site, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="b969f-176">![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784364.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="b969f-176">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784364.png "Security Controls")</span></span>

12. <span data-ttu-id="b969f-177">V hello **nastavení jednotného přihlašování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b969f-177">In hello **Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="b969f-178">![Jednotné přihlašování v nastavení](./media/active-directory-saas-jobscience-tutorial/ic781026.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="b969f-178">![Single Sign-On Settings](./media/active-directory-saas-jobscience-tutorial/ic781026.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="b969f-179">a.</span><span class="sxs-lookup"><span data-stu-id="b969f-179">a.</span></span> <span data-ttu-id="b969f-180">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="b969f-180">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="b969f-181">b.</span><span class="sxs-lookup"><span data-stu-id="b969f-181">b.</span></span> <span data-ttu-id="b969f-182">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="b969f-182">Click **New**.</span></span>

13. <span data-ttu-id="b969f-183">Na hello **SAML jeden přihlašování nastavení upravit** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b969f-183">On hello **SAML Single Sign-On Setting Edit** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="b969f-184">![SAML jeden přihlašování nastavení](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML jeden přihlašování nastavení")</span><span class="sxs-lookup"><span data-stu-id="b969f-184">![SAML Single Sign-On Setting](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="b969f-185">a.</span><span class="sxs-lookup"><span data-stu-id="b969f-185">a.</span></span> <span data-ttu-id="b969f-186">V hello **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="b969f-186">In hello **Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="b969f-187">b.</span><span class="sxs-lookup"><span data-stu-id="b969f-187">b.</span></span> <span data-ttu-id="b969f-188">V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b969f-188">In **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b969f-189">c.</span><span class="sxs-lookup"><span data-stu-id="b969f-189">c.</span></span> <span data-ttu-id="b969f-190">V hello **Entity Id** textovému poli, typu`https://salesforce-jobscience.com`</span><span class="sxs-lookup"><span data-stu-id="b969f-190">In hello **Entity Id** textbox, type `https://salesforce-jobscience.com`</span></span>

    <span data-ttu-id="b969f-191">d.</span><span class="sxs-lookup"><span data-stu-id="b969f-191">d.</span></span> <span data-ttu-id="b969f-192">Klikněte na tlačítko **Procházet** tooupload váš certifikát služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b969f-192">Click **Browse** tooupload your Azure AD certificate.</span></span>

    <span data-ttu-id="b969f-193">e.</span><span class="sxs-lookup"><span data-stu-id="b969f-193">e.</span></span> <span data-ttu-id="b969f-194">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje hello federace z objektu uživatele hello**.</span><span class="sxs-lookup"><span data-stu-id="b969f-194">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>

    <span data-ttu-id="b969f-195">f.</span><span class="sxs-lookup"><span data-stu-id="b969f-195">f.</span></span> <span data-ttu-id="b969f-196">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier hello hello subjektu příkaz**.</span><span class="sxs-lookup"><span data-stu-id="b969f-196">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>

    <span data-ttu-id="b969f-197">g.</span><span class="sxs-lookup"><span data-stu-id="b969f-197">g.</span></span> <span data-ttu-id="b969f-198">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b969f-198">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b969f-199">h.</span><span class="sxs-lookup"><span data-stu-id="b969f-199">h.</span></span> <span data-ttu-id="b969f-200">V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b969f-200">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="b969f-201">i.</span><span class="sxs-lookup"><span data-stu-id="b969f-201">i.</span></span> <span data-ttu-id="b969f-202">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b969f-202">Click **Save**.</span></span>

14. <span data-ttu-id="b969f-203">V levém navigačním podokně hello v hello **Správa** klikněte na tlačítko **Správa domén** tooexpand hello související části a pak klikněte na **Moje domény** tooopen hello  **Moje doména** stránky.</span><span class="sxs-lookup"><span data-stu-id="b969f-203">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="b969f-204">![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="b969f-204">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

15. <span data-ttu-id="b969f-205">Na hello **Moje domény** stránku hello **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="b969f-205">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="b969f-206">![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="b969f-206">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Login Page Branding")</span></span>

16. <span data-ttu-id="b969f-207">Na hello **Branding přihlašovací stránky** stránku hello **ověřovací služby** část, hello název vaší **nastavení jednotného přihlašování SAML** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b969f-207">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="b969f-208">Vyberte ji a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b969f-208">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="b969f-209">![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="b969f-209">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Login Page Branding")</span></span>

17. <span data-ttu-id="b969f-210">tooget hello SP zahájí jednotného přihlašování na adresu URL pro přihlášení, klikněte na hello **nastavení jednotného přihlašování** v hello **ovládacích prvků zabezpečení** části nabídky.</span><span class="sxs-lookup"><span data-stu-id="b969f-210">tooget hello SP initiated Single Sign on Login URL click on hello **Single Sign On settings** in hello **Security Controls** menu section.</span></span>

    <span data-ttu-id="b969f-211">![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784368.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="b969f-211">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784368.png "Security Controls")</span></span>
    
    <span data-ttu-id="b969f-212">Klikněte na tlačítko hello jednotného přihlašování k profilu, který jste vytvořili v předchozí krok text hello.</span><span class="sxs-lookup"><span data-stu-id="b969f-212">Click hello SSO profile you have created in hello step above.</span></span> <span data-ttu-id="b969f-213">Tato stránka zobrazuje hello jednotného přihlašování na adrese URL vaší společnosti (například [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span><span class="sxs-lookup"><span data-stu-id="b969f-213">This page shows hello Single Sign on URL for your company (for example, [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span></span>    

> [!TIP]
> <span data-ttu-id="b969f-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b969f-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b969f-215">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b969f-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b969f-216">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b969f-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b969f-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b969f-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="b969f-218">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b969f-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b969f-220">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b969f-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b969f-221">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b969f-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b969f-223">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b969f-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b969f-225">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b969f-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b969f-227">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b969f-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b969f-229">a.</span><span class="sxs-lookup"><span data-stu-id="b969f-229">a.</span></span> <span data-ttu-id="b969f-230">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b969f-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b969f-231">b.</span><span class="sxs-lookup"><span data-stu-id="b969f-231">b.</span></span> <span data-ttu-id="b969f-232">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b969f-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b969f-233">c.</span><span class="sxs-lookup"><span data-stu-id="b969f-233">c.</span></span> <span data-ttu-id="b969f-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b969f-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b969f-235">d.</span><span class="sxs-lookup"><span data-stu-id="b969f-235">d.</span></span> <span data-ttu-id="b969f-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b969f-236">Click **Create**.</span></span>
 
### <a name="creating-a-jobscience-test-user"></a><span data-ttu-id="b969f-237">Vytvoření zkušebního uživatele Jobscience</span><span class="sxs-lookup"><span data-stu-id="b969f-237">Creating a Jobscience test user</span></span>

<span data-ttu-id="b969f-238">V pořadí tooenable Azure AD Uživatelé toolog v tooJobscience musí být zřízená do Jobscience.</span><span class="sxs-lookup"><span data-stu-id="b969f-238">In order tooenable Azure AD users toolog in tooJobscience, they must be provisioned into Jobscience.</span></span> <span data-ttu-id="b969f-239">V případě hello Jobscience zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="b969f-239">In hello case of Jobscience, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="b969f-240">Můžete použít všechny ostatní Jobscience uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Jobscience tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="b969f-240">You can use any other Jobscience user account creation tools or APIs provided by Jobscience tooprovision Azure Active Directory user accounts.</span></span>
>  
        
<span data-ttu-id="b969f-241">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b969f-241">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="b969f-242">Přihlaste se tooyour **Jobscience** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="b969f-242">Log in tooyour **Jobscience** company site as administrator.</span></span>

2. <span data-ttu-id="b969f-243">Přejděte tooSetup.</span><span class="sxs-lookup"><span data-stu-id="b969f-243">Go tooSetup.</span></span>
   
   <span data-ttu-id="b969f-244">![Instalační program](./media/active-directory-saas-jobscience-tutorial/ic784358.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="b969f-244">![Setup](./media/active-directory-saas-jobscience-tutorial/ic784358.png "Setup")</span></span>
3. <span data-ttu-id="b969f-245">Přejděte příliš**spravovat uživatele \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b969f-245">Go too**Manage Users \> Users**.</span></span>
   
   <span data-ttu-id="b969f-246">![Uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784369.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="b969f-246">![Users](./media/active-directory-saas-jobscience-tutorial/ic784369.png "Users")</span></span>
4. <span data-ttu-id="b969f-247">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="b969f-247">Click **New User**.</span></span>
   
   <span data-ttu-id="b969f-248">![Všichni uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784370.png "všichni uživatelé")</span><span class="sxs-lookup"><span data-stu-id="b969f-248">![All Users](./media/active-directory-saas-jobscience-tutorial/ic784370.png "All Users")</span></span>
5. <span data-ttu-id="b969f-249">Na hello **upravit uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b969f-249">On hello **Edit User** dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="b969f-250">![Úprava uživatele](./media/active-directory-saas-jobscience-tutorial/ic784371.png "Úprava uživatele")</span><span class="sxs-lookup"><span data-stu-id="b969f-250">![User Edit](./media/active-directory-saas-jobscience-tutorial/ic784371.png "User Edit")</span></span>
   
   <span data-ttu-id="b969f-251">a.</span><span class="sxs-lookup"><span data-stu-id="b969f-251">a.</span></span> <span data-ttu-id="b969f-252">V hello **křestní jméno** textovému poli, zadejte jméno uživatele hello jako Britta.</span><span class="sxs-lookup"><span data-stu-id="b969f-252">In hello **First Name** textbox, type a first name of hello user like Britta.</span></span>
   
   <span data-ttu-id="b969f-253">b.</span><span class="sxs-lookup"><span data-stu-id="b969f-253">b.</span></span> <span data-ttu-id="b969f-254">V hello **příjmení** textovému poli, zadejte příjmení uživatele hello jako Simon.</span><span class="sxs-lookup"><span data-stu-id="b969f-254">In hello **Last Name** textbox, type a last name of hello user like Simon.</span></span>
   
   <span data-ttu-id="b969f-255">c.</span><span class="sxs-lookup"><span data-stu-id="b969f-255">c.</span></span> <span data-ttu-id="b969f-256">V hello **Alias** textovému poli, zadejte název aliasu hello uživatele jako brittas.</span><span class="sxs-lookup"><span data-stu-id="b969f-256">In hello **Alias** textbox, type an alias name of hello user like brittas.</span></span>

   <span data-ttu-id="b969f-257">d.</span><span class="sxs-lookup"><span data-stu-id="b969f-257">d.</span></span> <span data-ttu-id="b969f-258">V hello **e-mailu** jako typ hello e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b969f-258">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="b969f-259">e.</span><span class="sxs-lookup"><span data-stu-id="b969f-259">e.</span></span> <span data-ttu-id="b969f-260">V hello **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="b969f-260">In hello **User Name** textbox, type a user name of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="b969f-261">f.</span><span class="sxs-lookup"><span data-stu-id="b969f-261">f.</span></span> <span data-ttu-id="b969f-262">V hello **Přezdívka** textovému poli, zadejte název nick uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="b969f-262">In hello **Nick Name** textbox, type a nick name of user like Simon.</span></span>

   <span data-ttu-id="b969f-263">g.</span><span class="sxs-lookup"><span data-stu-id="b969f-263">g.</span></span> <span data-ttu-id="b969f-264">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b969f-264">Click **Save**.</span></span>

    
> [!NOTE]
> <span data-ttu-id="b969f-265">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="b969f-265">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b969f-266">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b969f-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b969f-267">V této části povolíte tak, že udělíte přístup tooJobscience toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b969f-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobscience.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b969f-269">**tooassign Britta Simon tooJobscience, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b969f-269">**tooassign Britta Simon tooJobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="b969f-270">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b969f-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b969f-272">V seznamu aplikace hello vyberte **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="b969f-272">In hello applications list, select **Jobscience**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. <span data-ttu-id="b969f-274">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b969f-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b969f-276">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b969f-276">Click **Add** button.</span></span> <span data-ttu-id="b969f-277">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b969f-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b969f-279">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b969f-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b969f-280">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b969f-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b969f-281">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b969f-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b969f-282">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b969f-282">Testing single sign-on</span></span>

<span data-ttu-id="b969f-283">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b969f-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b969f-284">Když kliknete na dlaždici Jobscience hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Jobscience aplikace.</span><span class="sxs-lookup"><span data-stu-id="b969f-284">When you click hello Jobscience tile in hello Access Panel, you should get automatically signed-on tooyour Jobscience application.</span></span>
<span data-ttu-id="b969f-285">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b969f-285">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b969f-286">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b969f-286">Additional resources</span></span>

* [<span data-ttu-id="b969f-287">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b969f-287">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b969f-288">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b969f-288">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

