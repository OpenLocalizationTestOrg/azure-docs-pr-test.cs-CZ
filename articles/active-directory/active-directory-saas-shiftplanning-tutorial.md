---
title: 'Kurz: Azure Active Directory integrace s lidskosti | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a lidskosti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7d8a04a2eb3c997f86f1e199c47809fa3dad60e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="5a656-103">Kurz: Azure Active Directory integrace s lidskosti</span><span class="sxs-lookup"><span data-stu-id="5a656-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="5a656-104">V tomto kurzu zjistíte, jak toointegrate lidskosti službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5a656-104">In this tutorial, you learn how toointegrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5a656-105">Integrace lidskosti s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5a656-105">Integrating Humanity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5a656-106">Můžete řídit ve službě Azure AD, který má přístup tooHumanity</span><span class="sxs-lookup"><span data-stu-id="5a656-106">You can control in Azure AD who has access tooHumanity</span></span>
- <span data-ttu-id="5a656-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHumanity (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a656-107">You can enable your users tooautomatically get signed-on tooHumanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5a656-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5a656-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5a656-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5a656-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a656-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5a656-110">Prerequisites</span></span>

<span data-ttu-id="5a656-111">Integrace služby Azure AD s lidskosti tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5a656-111">tooconfigure Azure AD integration with Humanity, you need hello following items:</span></span>

- <span data-ttu-id="5a656-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a656-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5a656-113">Lidskosti jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5a656-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5a656-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a656-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5a656-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5a656-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5a656-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5a656-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5a656-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5a656-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5a656-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5a656-118">Scenario description</span></span>
<span data-ttu-id="5a656-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5a656-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5a656-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5a656-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5a656-121">Přidání lidskosti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5a656-121">Adding Humanity from hello gallery</span></span>
2. <span data-ttu-id="5a656-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a656-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-hello-gallery"></a><span data-ttu-id="5a656-123">Přidání lidskosti z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5a656-123">Adding Humanity from hello gallery</span></span>
<span data-ttu-id="5a656-124">integrace hello tooconfigure lidskosti do služby Azure AD, je nutné tooadd lidskosti hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5a656-124">tooconfigure hello integration of Humanity into Azure AD, you need tooadd Humanity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5a656-125">**tooadd lidskosti z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a656-125">**tooadd Humanity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a656-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a656-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5a656-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5a656-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5a656-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a656-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5a656-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a656-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5a656-133">Hello vyhledávacího pole zadejte **lidskosti**.</span><span class="sxs-lookup"><span data-stu-id="5a656-133">In hello search box, type **Humanity**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="5a656-135">Na panelu výsledků hello vyberte **lidskosti**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a656-135">In hello results panel, select **Humanity**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5a656-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a656-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5a656-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s lidskosti podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5a656-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5a656-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v lidskosti je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5a656-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Humanity is tooa user in Azure AD.</span></span> <span data-ttu-id="5a656-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v lidskosti musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5a656-140">In other words, a link relationship between an Azure AD user and hello related user in Humanity needs toobe established.</span></span>

<span data-ttu-id="5a656-141">V lidskosti, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5a656-141">In Humanity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5a656-142">tooconfigure a testu Azure AD jednotné přihlašování s lidskosti, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5a656-142">tooconfigure and test Azure AD single sign-on with Humanity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5a656-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5a656-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5a656-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5a656-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5a656-145">**[Vytvoření zkušebního uživatele lidskosti](#creating-a-humanity-test-user)**  -toohave protějšek Britta Simon v lidskosti, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5a656-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - toohave a counterpart of Britta Simon in Humanity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5a656-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a656-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5a656-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5a656-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5a656-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a656-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5a656-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci lidskosti.</span><span class="sxs-lookup"><span data-stu-id="5a656-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="5a656-150">**tooconfigure Azure AD jednotné přihlašování s lidskosti, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a656-150">**tooconfigure Azure AD single sign-on with Humanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a656-151">V portálu Azure, na hello hello **lidskosti** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5a656-151">In hello Azure portal, on hello **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5a656-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a656-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="5a656-155">Na hello **lidskosti domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5a656-155">On hello **Humanity Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="5a656-157">a.</span><span class="sxs-lookup"><span data-stu-id="5a656-157">a.</span></span> <span data-ttu-id="5a656-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="5a656-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="5a656-159">b.</span><span class="sxs-lookup"><span data-stu-id="5a656-159">b.</span></span> <span data-ttu-id="5a656-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="5a656-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5a656-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5a656-161">These values are not real.</span></span> <span data-ttu-id="5a656-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5a656-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5a656-163">Obraťte se na [tým podpory lidskosti klienta](https://www.humanity.com/support/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5a656-163">Contact [Humanity Client support team](https://www.humanity.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="5a656-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5a656-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="5a656-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a656-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5a656-168">Na hello **lidskosti konfigurace** klikněte na tlačítko **konfigurace lidskosti** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5a656-168">On hello **Humanity Configuration** section, click **Configure Humanity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5a656-169">Kopírování hello **SAML jeden přihlašování adresa URL služby a adresu URL Sign-Out** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5a656-169">Copy hello **SAML Single Sign-On Service URL, and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="5a656-171">V okně prohlížeče jiných webových přihlásit tooyour **lidskosti** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="5a656-171">In a different web browser window, log in tooyour **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="5a656-172">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="5a656-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="5a656-173">![Správce](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "správce")</span><span class="sxs-lookup"><span data-stu-id="5a656-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="5a656-174">V části **integrace**, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5a656-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="5a656-175">![Jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5a656-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="5a656-176">V hello **jednotné přihlašování** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5a656-176">In hello **Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5a656-177">![Jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="5a656-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="5a656-178">a.</span><span class="sxs-lookup"><span data-stu-id="5a656-178">a.</span></span> <span data-ttu-id="5a656-179">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="5a656-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="5a656-180">b.</span><span class="sxs-lookup"><span data-stu-id="5a656-180">b.</span></span> <span data-ttu-id="5a656-181">Vyberte **povolit přihlášení heslo**.</span><span class="sxs-lookup"><span data-stu-id="5a656-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="5a656-182">c.</span><span class="sxs-lookup"><span data-stu-id="5a656-182">c.</span></span> <span data-ttu-id="5a656-183">Vložení hello **SAML jeden přihlašování adresa URL služby** hodnotu do hello **URL vystavitele SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a656-183">Paste hello **SAML Single Sign-On Service URL** value into hello **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="5a656-184">d.</span><span class="sxs-lookup"><span data-stu-id="5a656-184">d.</span></span> <span data-ttu-id="5a656-185">Vložení hello **Sign-Out URL** hodnotu do hello **vzdálené adresy URL odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a656-185">Paste hello **Sign-Out URL** value into hello **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="5a656-186">e.</span><span class="sxs-lookup"><span data-stu-id="5a656-186">e.</span></span> <span data-ttu-id="5a656-187">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5a656-187">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="5a656-188">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5a656-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="5a656-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5a656-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5a656-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5a656-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5a656-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5a656-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5a656-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5a656-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="5a656-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5a656-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5a656-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a656-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a656-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5a656-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5a656-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5a656-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5a656-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5a656-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5a656-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5a656-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5a656-204">a.</span><span class="sxs-lookup"><span data-stu-id="5a656-204">a.</span></span> <span data-ttu-id="5a656-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5a656-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5a656-206">b.</span><span class="sxs-lookup"><span data-stu-id="5a656-206">b.</span></span> <span data-ttu-id="5a656-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5a656-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5a656-208">c.</span><span class="sxs-lookup"><span data-stu-id="5a656-208">c.</span></span> <span data-ttu-id="5a656-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5a656-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5a656-210">d.</span><span class="sxs-lookup"><span data-stu-id="5a656-210">d.</span></span> <span data-ttu-id="5a656-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5a656-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="5a656-212">Vytvoření zkušebního uživatele lidskosti</span><span class="sxs-lookup"><span data-stu-id="5a656-212">Creating a Humanity test user</span></span>

<span data-ttu-id="5a656-213">V pořadí tooenable Azure AD Uživatelé toolog v tooHumanity musí být zřízená do lidskosti.</span><span class="sxs-lookup"><span data-stu-id="5a656-213">In order tooenable Azure AD users toolog in tooHumanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="5a656-214">V případě hello lidskosti zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="5a656-214">In hello case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="5a656-215">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a656-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a656-216">Přihlaste se tooyour **lidskosti** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="5a656-216">Log in tooyour **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="5a656-217">Klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="5a656-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="5a656-218">![Správce](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "správce")</span><span class="sxs-lookup"><span data-stu-id="5a656-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="5a656-219">Klikněte na tlačítko **zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="5a656-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="5a656-220">![Zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="5a656-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="5a656-221">V části **akce**, klikněte na tlačítko **přidat zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="5a656-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="5a656-222">![Přidat zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "přidat zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="5a656-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="5a656-223">V hello **přidat zaměstnanci** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5a656-223">In hello **Add Employees** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="5a656-224">![Uložit zaměstnanci](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "uložit zaměstnanci")</span><span class="sxs-lookup"><span data-stu-id="5a656-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="5a656-225">a.</span><span class="sxs-lookup"><span data-stu-id="5a656-225">a.</span></span> <span data-ttu-id="5a656-226">Typ hello **křestní jméno**, **příjmení**, a **e-mailu** z platný účet AAD chcete tooprovision do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="5a656-226">Type hello **First Name**, **Last Name**, and **Email** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="5a656-227">b.</span><span class="sxs-lookup"><span data-stu-id="5a656-227">b.</span></span> <span data-ttu-id="5a656-228">Klikněte na tlačítko **uložit zaměstnanci**.</span><span class="sxs-lookup"><span data-stu-id="5a656-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="5a656-229">Můžete použít všechny ostatní lidskosti uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované lidskosti tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="5a656-229">You can use any other Humanity user account creation tools or APIs provided by Humanity tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5a656-230">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5a656-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5a656-231">V této části povolíte tak, že udělíte přístup tooHumanity toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5a656-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHumanity.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5a656-233">**tooassign Britta Simon tooHumanity, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5a656-233">**tooassign Britta Simon tooHumanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="5a656-234">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5a656-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5a656-236">V seznamu aplikace hello vyberte **lidskosti**.</span><span class="sxs-lookup"><span data-stu-id="5a656-236">In hello applications list, select **Humanity**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="5a656-238">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5a656-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5a656-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5a656-240">Click **Add** button.</span></span> <span data-ttu-id="5a656-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a656-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5a656-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5a656-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5a656-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a656-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5a656-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5a656-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5a656-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5a656-246">Testing single sign-on</span></span>

<span data-ttu-id="5a656-247">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5a656-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5a656-248">Když kliknete na dlaždici lidskosti hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour lidskosti aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a656-248">When you click hello Humanity tile in hello Access Panel, you should get automatically signed-on tooyour Humanity application.</span></span>
<span data-ttu-id="5a656-249">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5a656-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a656-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5a656-250">Additional resources</span></span>

* [<span data-ttu-id="5a656-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5a656-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5a656-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5a656-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

