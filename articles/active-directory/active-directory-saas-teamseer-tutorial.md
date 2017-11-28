---
title: 'Kurz: Azure Active Directory integrace s TeamSeer | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="7db31-103">Kurz: Azure Active Directory integrace s TeamSeer</span><span class="sxs-lookup"><span data-stu-id="7db31-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="7db31-104">V tomto kurzu zjistíte, jak toointegrate TeamSeer s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7db31-104">In this tutorial, you learn how toointegrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7db31-105">Integrace TeamSeer s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7db31-105">Integrating TeamSeer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7db31-106">Můžete řídit ve službě Azure AD, který má přístup tooTeamSeer</span><span class="sxs-lookup"><span data-stu-id="7db31-106">You can control in Azure AD who has access tooTeamSeer</span></span>
- <span data-ttu-id="7db31-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTeamSeer (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7db31-107">You can enable your users tooautomatically get signed-on tooTeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7db31-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7db31-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7db31-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7db31-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7db31-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7db31-110">Prerequisites</span></span>

<span data-ttu-id="7db31-111">Integrace služby Azure AD s TeamSeer tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7db31-111">tooconfigure Azure AD integration with TeamSeer, you need hello following items:</span></span>

- <span data-ttu-id="7db31-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7db31-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7db31-113">TeamSeer jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7db31-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7db31-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7db31-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7db31-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7db31-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7db31-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7db31-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7db31-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7db31-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7db31-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7db31-118">Scenario description</span></span>
<span data-ttu-id="7db31-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7db31-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7db31-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7db31-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7db31-121">Přidání TeamSeer z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7db31-121">Adding TeamSeer from hello gallery</span></span>
2. <span data-ttu-id="7db31-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7db31-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-hello-gallery"></a><span data-ttu-id="7db31-123">Přidání TeamSeer z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7db31-123">Adding TeamSeer from hello gallery</span></span>
<span data-ttu-id="7db31-124">tooconfigure hello integrace TeamSeer v tooAzure AD, je nutné tooadd TeamSeer hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7db31-124">tooconfigure hello integration of TeamSeer in tooAzure AD, you need tooadd TeamSeer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7db31-125">**tooadd TeamSeer z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7db31-125">**tooadd TeamSeer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7db31-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7db31-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7db31-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7db31-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7db31-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7db31-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7db31-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7db31-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7db31-133">Hello vyhledávacího pole zadejte **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="7db31-133">In hello search box, type **TeamSeer**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="7db31-135">Na panelu výsledků hello vyberte **TeamSeer**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7db31-135">In hello results panel, select **TeamSeer**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7db31-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7db31-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7db31-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TeamSeer podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="7db31-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7db31-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v TeamSeer je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7db31-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TeamSeer is tooa user in Azure AD.</span></span> <span data-ttu-id="7db31-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v TeamSeer musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7db31-140">In other words, a link relationship between an Azure AD user and hello related user in TeamSeer needs toobe established.</span></span>

<span data-ttu-id="7db31-141">V TeamSeer, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7db31-141">In TeamSeer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7db31-142">tooconfigure a testu Azure AD jednotné přihlašování s TeamSeer, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7db31-142">tooconfigure and test Azure AD single sign-on with TeamSeer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7db31-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7db31-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7db31-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7db31-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7db31-145">**[Vytvoření zkušebního uživatele TeamSeer](#creating-a-teamseer-test-user)**  -toohave protějšek Britta Simon v TeamSeer, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7db31-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - toohave a counterpart of Britta Simon in TeamSeer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7db31-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7db31-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7db31-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7db31-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7db31-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7db31-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7db31-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci TeamSeer.</span><span class="sxs-lookup"><span data-stu-id="7db31-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="7db31-150">**tooconfigure Azure AD jednotné přihlašování s TeamSeer, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7db31-150">**tooconfigure Azure AD single sign-on with TeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="7db31-151">V portálu Azure, na hello hello **TeamSeer** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7db31-151">In hello Azure portal, on hello **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7db31-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7db31-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="7db31-155">Na hello **TeamSeer domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7db31-155">On hello **TeamSeer Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="7db31-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="7db31-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7db31-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="7db31-158">hello value is not real.</span></span> <span data-ttu-id="7db31-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7db31-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7db31-160">Obraťte se na [tým podpory TeamSeer klienta](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7db31-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello value.</span></span> 
 
4. <span data-ttu-id="7db31-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7db31-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="7db31-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7db31-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7db31-165">Na hello **TeamSeer konfigurace** klikněte na tlačítko **konfigurace TeamSeer** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7db31-165">On hello **TeamSeer Configuration** section, click **Configure TeamSeer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7db31-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7db31-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="7db31-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti TeamSeer tooyour.</span><span class="sxs-lookup"><span data-stu-id="7db31-168">In a different web browser window, log in tooyour TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="7db31-169">Přejděte příliš**HR správce**.</span><span class="sxs-lookup"><span data-stu-id="7db31-169">Go too**HR Admin**.</span></span>
   
    <span data-ttu-id="7db31-170">![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR správce")</span><span class="sxs-lookup"><span data-stu-id="7db31-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="7db31-171">Klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="7db31-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="7db31-172">![Instalační program](./media/active-directory-saas-teamseer-tutorial/ic789635.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="7db31-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="7db31-173">Klikněte na tlačítko **nastavit podrobné informace o poskytovateli SAML**.</span><span class="sxs-lookup"><span data-stu-id="7db31-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="7db31-174">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="7db31-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="7db31-175">V hello v části Podrobnosti o zprostředkovatele SAML proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7db31-175">In hello SAML provider details section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7db31-176">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="7db31-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="7db31-177">a.</span><span class="sxs-lookup"><span data-stu-id="7db31-177">a.</span></span> <span data-ttu-id="7db31-178">Vložení hello **jeden přihlašování adresa URL služby** hodnota v toohello **URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7db31-178">Paste hello **Single Sign-On Service URL** value in toohello **URL** textbox.</span></span>
          
    <span data-ttu-id="7db31-179">b.</span><span class="sxs-lookup"><span data-stu-id="7db31-179">b.</span></span> <span data-ttu-id="7db31-180">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky tooyour a pak ji vložit toohello **veřejný certifikát IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7db31-180">Open your base-64 encoded certificate in notepad, copy hello content of it in tooyour clipboard, and then paste it toohello **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="7db31-181">toocomplete hello zprostředkovatele konfigurace SAML, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7db31-181">toocomplete hello SAML provider configuration, perform hello following steps:</span></span>
    
    <span data-ttu-id="7db31-182">![Nastavení SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "nastavení SAML")</span><span class="sxs-lookup"><span data-stu-id="7db31-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="7db31-183">a.</span><span class="sxs-lookup"><span data-stu-id="7db31-183">a.</span></span> <span data-ttu-id="7db31-184">V hello **testovací e-mailové adresy**, zadejte hello testovacího uživatele e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="7db31-184">In hello **Test Email Addresses**, type hello test user’s email address.</span></span> 
  
    <span data-ttu-id="7db31-185">b.</span><span class="sxs-lookup"><span data-stu-id="7db31-185">b.</span></span> <span data-ttu-id="7db31-186">V hello **vystavitele** textovému poli, typ hello URL vystavitele hello poskytovatele služeb.</span><span class="sxs-lookup"><span data-stu-id="7db31-186">In hello **Issuer** textbox, type hello Issuer URL of hello service provider.</span></span> 
  
    <span data-ttu-id="7db31-187">c.</span><span class="sxs-lookup"><span data-stu-id="7db31-187">c.</span></span> <span data-ttu-id="7db31-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7db31-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7db31-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7db31-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7db31-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7db31-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7db31-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7db31-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7db31-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7db31-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="7db31-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7db31-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7db31-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7db31-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7db31-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7db31-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7db31-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7db31-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7db31-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7db31-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7db31-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7db31-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7db31-204">a.</span><span class="sxs-lookup"><span data-stu-id="7db31-204">a.</span></span> <span data-ttu-id="7db31-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7db31-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7db31-206">b.</span><span class="sxs-lookup"><span data-stu-id="7db31-206">b.</span></span> <span data-ttu-id="7db31-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7db31-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7db31-208">c.</span><span class="sxs-lookup"><span data-stu-id="7db31-208">c.</span></span> <span data-ttu-id="7db31-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7db31-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7db31-210">d.</span><span class="sxs-lookup"><span data-stu-id="7db31-210">d.</span></span> <span data-ttu-id="7db31-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7db31-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="7db31-212">Vytvoření zkušebního uživatele TeamSeer</span><span class="sxs-lookup"><span data-stu-id="7db31-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="7db31-213">Uživatelé toolog tooenable Azure AD v tooTeamSeer, se musí být zřízená v tooShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="7db31-213">tooenable Azure AD users toolog in tooTeamSeer, they must be provisioned in tooShiftPlanning.</span></span> <span data-ttu-id="7db31-214">V případě hello TeamSeer zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7db31-214">In hello case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="7db31-215">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7db31-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="7db31-216">Přihlaste se tooyour **TeamSeer** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="7db31-216">Log in tooyour **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="7db31-217">Proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7db31-217">Perform hello following steps:</span></span>
   
    <span data-ttu-id="7db31-218">![Správce HR](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR správce")</span><span class="sxs-lookup"><span data-stu-id="7db31-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="7db31-219">a.</span><span class="sxs-lookup"><span data-stu-id="7db31-219">a.</span></span> <span data-ttu-id="7db31-220">Přejděte příliš**HR správce \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7db31-220">Go too**HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="7db31-221">b.</span><span class="sxs-lookup"><span data-stu-id="7db31-221">b.</span></span> <span data-ttu-id="7db31-222">Klikněte na tlačítko **spusťte Průvodce nového uživatele hello**.</span><span class="sxs-lookup"><span data-stu-id="7db31-222">Click **Run hello New User wizard**.</span></span>

3. <span data-ttu-id="7db31-223">V hello **podrobné informace o uživateli** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7db31-223">In hello **User Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7db31-224">![Podrobné informace o uživateli](./media/active-directory-saas-teamseer-tutorial/ic789641.png "podrobné informace o uživateli")</span><span class="sxs-lookup"><span data-stu-id="7db31-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="7db31-225">a.</span><span class="sxs-lookup"><span data-stu-id="7db31-225">a.</span></span> <span data-ttu-id="7db31-226">Typ hello **křestní jméno**, **Přezdívka**, **uživatelské jméno (e-mailovou adresu)** z platný účet AAD chcete tooprovision v toohello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="7db31-226">Type hello **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want tooprovision in toohello related textboxes.</span></span>
  
    <span data-ttu-id="7db31-227">b.</span><span class="sxs-lookup"><span data-stu-id="7db31-227">b.</span></span> <span data-ttu-id="7db31-228">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="7db31-228">Click **Next**.</span></span>

4. <span data-ttu-id="7db31-229">Postupujte podle hello na obrazovce pokyny pro přidání nového uživatele a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="7db31-229">Follow hello on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="7db31-230">Můžete použít všechny ostatní TeamSeer uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TeamSeer tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7db31-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7db31-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7db31-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7db31-232">V této části povolíte tak, že udělíte přístup tooTeamSeer toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7db31-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTeamSeer.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7db31-234">**tooassign Britta Simon tooTeamSeer, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7db31-234">**tooassign Britta Simon tooTeamSeer, perform hello following steps:**</span></span>

1. <span data-ttu-id="7db31-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7db31-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7db31-237">V seznamu aplikace hello vyberte **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="7db31-237">In hello applications list, select **TeamSeer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="7db31-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7db31-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7db31-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7db31-241">Click **Add** button.</span></span> <span data-ttu-id="7db31-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7db31-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7db31-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7db31-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7db31-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7db31-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7db31-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7db31-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7db31-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7db31-247">Testing single sign-on</span></span>

<span data-ttu-id="7db31-248">Pokud chcete tootest vaše nastavení jednotného přihlašování, otevřete hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7db31-248">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="7db31-249">Další podrobnosti o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7db31-249">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7db31-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7db31-250">Additional resources</span></span>

* [<span data-ttu-id="7db31-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7db31-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7db31-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7db31-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

