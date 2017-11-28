---
title: 'Kurz: Azure Active Directory integrace s Halogen softwarem | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Halogen softwarem a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bdb67de713771d6e306f287c4b13895f6336f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a><span data-ttu-id="ceb32-103">Kurz: Azure Active Directory integrace s Halogen softwaru</span><span class="sxs-lookup"><span data-stu-id="ceb32-103">Tutorial: Azure Active Directory integration with Halogen Software</span></span>

<span data-ttu-id="ceb32-104">V tomto kurzu zjistíte, jak toointegrate Halogen Software s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ceb32-104">In this tutorial, you learn how toointegrate Halogen Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ceb32-105">Integrace Halogen softwaru s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ceb32-105">Integrating Halogen Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ceb32-106">Můžete řídit ve službě Azure AD, který má přístup tooHalogen softwaru</span><span class="sxs-lookup"><span data-stu-id="ceb32-106">You can control in Azure AD who has access tooHalogen Software</span></span>
- <span data-ttu-id="ceb32-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHalogen softwaru (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="ceb32-107">You can enable your users tooautomatically get signed-on tooHalogen Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ceb32-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ceb32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ceb32-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ceb32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ceb32-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ceb32-110">Prerequisites</span></span>

<span data-ttu-id="ceb32-111">tooconfigure integrace Azure AD s Halogen softwaru, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ceb32-111">tooconfigure Azure AD integration with Halogen Software, you need hello following items:</span></span>

- <span data-ttu-id="ceb32-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="ceb32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ceb32-113">Softwaru Halogen jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="ceb32-113">A Halogen Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ceb32-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ceb32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ceb32-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="ceb32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ceb32-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="ceb32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ceb32-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ceb32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ceb32-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ceb32-118">Scenario description</span></span>

<span data-ttu-id="ceb32-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ceb32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ceb32-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ceb32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ceb32-121">Přidání softwaru Halogen z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ceb32-121">Adding Halogen Software from hello gallery</span></span>
2. <span data-ttu-id="ceb32-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ceb32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-halogen-software-from-hello-gallery"></a><span data-ttu-id="ceb32-123">Přidání softwaru Halogen z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="ceb32-123">Adding Halogen Software from hello gallery</span></span>

<span data-ttu-id="ceb32-124">tooconfigure hello integrace Halogen softwaru do služby Azure AD, je nutné tooadd Halogen softwaru hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="ceb32-124">tooconfigure hello integration of Halogen Software into Azure AD, you need tooadd Halogen Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ceb32-125">**tooadd Halogen softwaru z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ceb32-125">**tooadd Halogen Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceb32-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ceb32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ceb32-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ceb32-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ceb32-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ceb32-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ceb32-133">Hello vyhledávacího pole zadejte **Halogen softwaru**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-133">In hello search box, type **Halogen Software**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. <span data-ttu-id="ceb32-135">Na panelu výsledků hello vyberte **Halogen softwaru**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ceb32-135">In hello results panel, select **Halogen Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ceb32-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ceb32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ceb32-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Halogen Software založený na testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ceb32-138">In this section, you configure and test Azure AD single sign-on with Halogen Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ceb32-139">Pro toowork jeden přihlašování Azure AD musí tooknow uživatele hello protějškem v Halogen softwaru je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ceb32-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halogen Software is tooa user in Azure AD.</span></span> <span data-ttu-id="ceb32-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v softwaru Halogen musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="ceb32-140">In other words, a link relationship between an Azure AD user and hello related user in Halogen Software needs toobe established.</span></span>

<span data-ttu-id="ceb32-141">V softwaru Halogen přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="ceb32-141">In Halogen Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ceb32-142">tooconfigure a testu Azure AD jednotné přihlašování s Halogen softwaru, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="ceb32-142">tooconfigure and test Azure AD single sign-on with Halogen Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ceb32-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="ceb32-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ceb32-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ceb32-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ceb32-145">**[Vytvoření zkušebního uživatele Halogen softwaru](#creating-a-halogen-software-test-user)**  -toohave protějšek Britta Simon v Halogen Software, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="ceb32-145">**[Creating a Halogen Software test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in Halogen Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ceb32-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ceb32-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ceb32-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="ceb32-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ceb32-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ceb32-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ceb32-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Halogen softwaru.</span><span class="sxs-lookup"><span data-stu-id="ceb32-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Halogen Software application.</span></span>

<span data-ttu-id="ceb32-150">**tooconfigure Azure AD jednotné přihlašování s Halogen softwarem, provést hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ceb32-150">**tooconfigure Azure AD single sign-on with Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceb32-151">V portálu Azure, na hello hello **Halogen softwaru** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-151">In hello Azure portal, on hello **Halogen Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ceb32-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ceb32-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. <span data-ttu-id="ceb32-155">Na hello **Halogen softwaru domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="ceb32-155">On hello **Halogen Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    <span data-ttu-id="ceb32-157">a.</span><span class="sxs-lookup"><span data-stu-id="ceb32-157">a.</span></span> <span data-ttu-id="ceb32-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="ceb32-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://global.hgncloud.com/<companyname>`</span></span>

    <span data-ttu-id="ceb32-159">b.</span><span class="sxs-lookup"><span data-stu-id="ceb32-159">b.</span></span> <span data-ttu-id="ceb32-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="ceb32-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ceb32-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="ceb32-161">These values are not real.</span></span> <span data-ttu-id="ceb32-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ceb32-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ceb32-163">Obraťte se na [tým podpory klientský Software Halogen](https://support.halogensoftware.com/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ceb32-163">Contact [Halogen Software Client support team](https://support.halogensoftware.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="ceb32-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ceb32-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. <span data-ttu-id="ceb32-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ceb32-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ceb32-168">V okně jiný prohlížeč, přihlášení tooyour **Halogen softwaru** aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="ceb32-168">In a different browser window, sign-on tooyour **Halogen Software** application as an administrator.</span></span>

7. <span data-ttu-id="ceb32-169">Klikněte na tlačítko hello **možnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="ceb32-169">Click hello **Options** tab.</span></span> 
   
    ![Co je služba Azure AD Connect][12]

8. <span data-ttu-id="ceb32-171">V levém navigačním podokně hello, klikněte na **konfigurace SAML**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-171">In hello left navigation pane, click **SAML Configuration**.</span></span> 
   
    ![Co je služba Azure AD Connect][13]

9. <span data-ttu-id="ceb32-173">Na hello **konfigurace SAML** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ceb32-173">On hello **SAML Configuration** page, perform hello following steps:</span></span> 

    ![Co je služba Azure AD Connect][14]

     <span data-ttu-id="ceb32-175">a.</span><span class="sxs-lookup"><span data-stu-id="ceb32-175">a.</span></span> <span data-ttu-id="ceb32-176">Jako **jedinečný identifikátor**, vyberte **NameID**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-176">As **Unique Identifier**, select **NameID**.</span></span>

     <span data-ttu-id="ceb32-177">b.</span><span class="sxs-lookup"><span data-stu-id="ceb32-177">b.</span></span> <span data-ttu-id="ceb32-178">Jako **mapy jedinečný identifikátor pro**, vyberte **uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-178">As **Unique Identifier Maps To**, select **Username**.</span></span>
  
     <span data-ttu-id="ceb32-179">c.</span><span class="sxs-lookup"><span data-stu-id="ceb32-179">c.</span></span> <span data-ttu-id="ceb32-180">tooupload váš soubor stažený metadata, klikněte na tlačítko **Procházet** tooselect hello souboru a potom **nahrát soubor**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-180">tooupload your downloaded metadata file, click **Browse** tooselect hello file, and then **Upload File**.</span></span>
 
     <span data-ttu-id="ceb32-181">d.</span><span class="sxs-lookup"><span data-stu-id="ceb32-181">d.</span></span> <span data-ttu-id="ceb32-182">tootest hello konfiguraci, klikněte na tlačítko **spustit Test**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-182">tootest hello configuration, click **Run Test**.</span></span> 
    
    >[!NOTE]
    ><span data-ttu-id="ceb32-183">Potřebujete toowait uvítací zprávu "*hello SAML test je dokončen. Zavřete toto okno*".</span><span class="sxs-lookup"><span data-stu-id="ceb32-183">You need toowait for hello message "*hello SAML test is complete. Please close this window*".</span></span> <span data-ttu-id="ceb32-184">Zavřete okno prohlížeče otevřenou hello.</span><span class="sxs-lookup"><span data-stu-id="ceb32-184">Then, close hello opened browser window.</span></span> <span data-ttu-id="ceb32-185">Hello **povolit SAML** zaškrtávací políčko je povoleno pouze v případě hello test byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="ceb32-185">hello **Enable SAML** checkbox is only enabled if hello test has been completed.</span></span> 
     
     <span data-ttu-id="ceb32-186">e.</span><span class="sxs-lookup"><span data-stu-id="ceb32-186">e.</span></span> <span data-ttu-id="ceb32-187">Vyberte **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-187">Select **Enable SAML**.</span></span>
    
     <span data-ttu-id="ceb32-188">f.</span><span class="sxs-lookup"><span data-stu-id="ceb32-188">f.</span></span> <span data-ttu-id="ceb32-189">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-189">Click **Save Changes**.</span></span> 

> [!TIP]
> <span data-ttu-id="ceb32-190">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="ceb32-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ceb32-191">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="ceb32-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ceb32-192">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ceb32-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ceb32-193">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ceb32-193">Creating an Azure AD test user</span></span>

<span data-ttu-id="ceb32-194">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="ceb32-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ceb32-196">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ceb32-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceb32-197">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ceb32-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ceb32-199">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ceb32-201">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="ceb32-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ceb32-203">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ceb32-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ceb32-205">a.</span><span class="sxs-lookup"><span data-stu-id="ceb32-205">a.</span></span> <span data-ttu-id="ceb32-206">V hello **název** textovému poli, název typu jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-206">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="ceb32-207">b.</span><span class="sxs-lookup"><span data-stu-id="ceb32-207">b.</span></span> <span data-ttu-id="ceb32-208">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ceb32-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ceb32-209">c.</span><span class="sxs-lookup"><span data-stu-id="ceb32-209">c.</span></span> <span data-ttu-id="ceb32-210">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ceb32-211">d.</span><span class="sxs-lookup"><span data-stu-id="ceb32-211">d.</span></span> <span data-ttu-id="ceb32-212">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-212">Click **Create**.</span></span>
 
### <a name="creating-a-halogen-software-test-user"></a><span data-ttu-id="ceb32-213">Vytvoření zkušebního uživatele Halogen softwaru</span><span class="sxs-lookup"><span data-stu-id="ceb32-213">Creating a Halogen Software test user</span></span>

<span data-ttu-id="ceb32-214">Hello cílem této části je toocreate uživatel volal Britta Simon v Halogen softwaru.</span><span class="sxs-lookup"><span data-stu-id="ceb32-214">hello objective of this section is toocreate a user called Britta Simon in Halogen Software.</span></span>

<span data-ttu-id="ceb32-215">**toocreate uživatel volal Britta Simon v Halogen Software, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ceb32-215">**toocreate a user called Britta Simon in Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceb32-216">Přihlaste se tooyour **Halogen softwaru** aplikace jako správce.</span><span class="sxs-lookup"><span data-stu-id="ceb32-216">Sign on tooyour **Halogen Software** application as an administrator.</span></span>

2. <span data-ttu-id="ceb32-217">Klikněte na tlačítko hello **uživatele Center** a pak klikněte **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-217">Click hello **User Center** tab, and then click **Create User**.</span></span>
   
    ![Co je služba Azure AD Connect][300]  

3. <span data-ttu-id="ceb32-219">Na hello **nového uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ceb32-219">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    ![Co je služba Azure AD Connect][301]

    <span data-ttu-id="ceb32-221">a.</span><span class="sxs-lookup"><span data-stu-id="ceb32-221">a.</span></span> <span data-ttu-id="ceb32-222">V hello **křestní jméno** textovému poli, typ křestní jméno uživatele hello jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-222">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>
    
    <span data-ttu-id="ceb32-223">b.</span><span class="sxs-lookup"><span data-stu-id="ceb32-223">b.</span></span> <span data-ttu-id="ceb32-224">V hello **příjmení** textovému poli, typ příjmení uživatele hello jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-224">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span> 

    <span data-ttu-id="ceb32-225">c.</span><span class="sxs-lookup"><span data-stu-id="ceb32-225">c.</span></span> <span data-ttu-id="ceb32-226">V hello **uživatelské jméno** textovému poli, typ **Britta Simon**, hello uživatelské jméno jako hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb32-226">In hello **Username** textbox, type **Britta Simon**, hello user name as in hello Azure portal.</span></span>

    <span data-ttu-id="ceb32-227">d.</span><span class="sxs-lookup"><span data-stu-id="ceb32-227">d.</span></span> <span data-ttu-id="ceb32-228">V hello **heslo** textovému poli, zadejte heslo pro Britta.</span><span class="sxs-lookup"><span data-stu-id="ceb32-228">In hello **Password** textbox, type a password for Britta.</span></span>
    
    <span data-ttu-id="ceb32-229">e.</span><span class="sxs-lookup"><span data-stu-id="ceb32-229">e.</span></span> <span data-ttu-id="ceb32-230">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-230">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ceb32-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="ceb32-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ceb32-232">V této části povolíte tak, že udělíte přístup tooHalogen softwaru Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ceb32-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHalogen Software.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ceb32-234">**tooassign Britta Simon tooHalogen softwaru, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="ceb32-234">**tooassign Britta Simon tooHalogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="ceb32-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ceb32-237">V seznamu aplikace hello vyberte **Halogen softwaru**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-237">In hello applications list, select **Halogen Software**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. <span data-ttu-id="ceb32-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ceb32-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ceb32-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ceb32-241">Click **Add** button.</span></span> <span data-ttu-id="ceb32-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ceb32-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ceb32-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="ceb32-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ceb32-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ceb32-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ceb32-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ceb32-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ceb32-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ceb32-247">Testing single sign-on</span></span>

<span data-ttu-id="ceb32-248">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="ceb32-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="ceb32-249">Když kliknete na dlaždici Halogen softwaru hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Halogen softwarová aplikace.</span><span class="sxs-lookup"><span data-stu-id="ceb32-249">When you click hello Halogen Software tile in hello Access Panel, you should get automatically signed-on tooyour Halogen Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ceb32-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ceb32-250">Additional resources</span></span>

* [<span data-ttu-id="ceb32-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ceb32-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ceb32-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ceb32-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
