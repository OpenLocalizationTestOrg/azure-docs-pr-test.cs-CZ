---
title: "Kurz: Azure Active Directory integrace s malé vylepšení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi malé vylepšení a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 59c8a112-41e1-4337-9ef3-3d7029780d61
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 33213fe4b61f5005cf78bee2c05b2b1e5e71ae8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-small-improvements"></a><span data-ttu-id="dac42-103">Kurz: Azure Active Directory integrace s malé vylepšení</span><span class="sxs-lookup"><span data-stu-id="dac42-103">Tutorial: Azure Active Directory integration with Small Improvements</span></span>

<span data-ttu-id="dac42-104">V tomto kurzu zjistíte, jak toointegrate malé vylepšení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dac42-104">In this tutorial, you learn how toointegrate Small Improvements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dac42-105">Malé vylepšení integrace s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dac42-105">Integrating Small Improvements with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dac42-106">Můžete řídit ve službě Azure AD, který má přístup tooSmall vylepšení</span><span class="sxs-lookup"><span data-stu-id="dac42-106">You can control in Azure AD who has access tooSmall Improvements</span></span>
- <span data-ttu-id="dac42-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSmall vylepšení (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dac42-107">You can enable your users tooautomatically get signed-on tooSmall Improvements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dac42-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dac42-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="dac42-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dac42-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dac42-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dac42-110">Prerequisites</span></span>

<span data-ttu-id="dac42-111">tooconfigure integrace Azure AD s malé vylepšení, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="dac42-111">tooconfigure Azure AD integration with Small Improvements, you need hello following items:</span></span>

- <span data-ttu-id="dac42-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dac42-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dac42-113">Malých vylepšení jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dac42-113">A Small Improvements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dac42-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dac42-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dac42-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dac42-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dac42-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dac42-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dac42-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dac42-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dac42-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dac42-118">Scenario description</span></span>
<span data-ttu-id="dac42-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dac42-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dac42-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dac42-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dac42-121">Přidání vylepšení malé z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dac42-121">Adding Small Improvements from hello gallery</span></span>
2. <span data-ttu-id="dac42-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dac42-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-small-improvements-from-hello-gallery"></a><span data-ttu-id="dac42-123">Přidání vylepšení malé z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="dac42-123">Adding Small Improvements from hello gallery</span></span>
<span data-ttu-id="dac42-124">tooconfigure hello integrace malé vylepšení do Azure AD, je nutné tooadd malé vylepšení hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dac42-124">tooconfigure hello integration of Small Improvements into Azure AD, you need tooadd Small Improvements from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dac42-125">**tooadd malé vylepšení z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dac42-125">**tooadd Small Improvements from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dac42-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dac42-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dac42-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dac42-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="dac42-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dac42-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dac42-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dac42-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dac42-133">Hello vyhledávacího pole zadejte **malé vylepšení**.</span><span class="sxs-lookup"><span data-stu-id="dac42-133">In hello search box, type **Small Improvements**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_search.png)

5. <span data-ttu-id="dac42-135">Na panelu výsledků hello vyberte **malé vylepšení**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="dac42-135">In hello results panel, select **Small Improvements**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dac42-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dac42-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dac42-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s malé vylepšení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="dac42-138">In this section, you configure and test Azure AD single sign-on with Small Improvements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dac42-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v malých vylepšení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dac42-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Small Improvements is tooa user in Azure AD.</span></span> <span data-ttu-id="dac42-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v malých vylepšení musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="dac42-140">In other words, a link relationship between an Azure AD user and hello related user in Small Improvements needs toobe established.</span></span>

<span data-ttu-id="dac42-141">V malých vylepšení přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="dac42-141">In Small Improvements, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="dac42-142">tooconfigure a testu Azure AD jednotné přihlašování s malé vylepšení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="dac42-142">tooconfigure and test Azure AD single sign-on with Small Improvements, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dac42-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="dac42-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dac42-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dac42-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dac42-145">**[Vytvoření zkušebního uživatele malé vylepšení](#creating-a-small-improvements-test-user)**  -toohave protějšek Britta Simon v malých vylepšení, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="dac42-145">**[Creating a Small Improvements test user](#creating-a-small-improvements-test-user)** - toohave a counterpart of Britta Simon in Small Improvements that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="dac42-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dac42-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dac42-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="dac42-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dac42-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dac42-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dac42-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci malé vylepšení.</span><span class="sxs-lookup"><span data-stu-id="dac42-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Small Improvements application.</span></span>

<span data-ttu-id="dac42-150">**tooconfigure Azure AD jednotné přihlašování s malé vylepšení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dac42-150">**tooconfigure Azure AD single sign-on with Small Improvements, perform hello following steps:**</span></span>

1. <span data-ttu-id="dac42-151">V portálu Azure, na hello hello **malé vylepšení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dac42-151">In hello Azure portal, on hello **Small Improvements** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dac42-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dac42-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_samlbase.png)

3. <span data-ttu-id="dac42-155">Na hello **malé vylepšení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dac42-155">On hello **Small Improvements Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_url.png)

    <span data-ttu-id="dac42-157">a.</span><span class="sxs-lookup"><span data-stu-id="dac42-157">a.</span></span> <span data-ttu-id="dac42-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="dac42-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    <span data-ttu-id="dac42-159">b.</span><span class="sxs-lookup"><span data-stu-id="dac42-159">b.</span></span> <span data-ttu-id="dac42-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<subdomain>.small-improvements.com`</span><span class="sxs-lookup"><span data-stu-id="dac42-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.small-improvements.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dac42-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="dac42-161">These values are not real.</span></span> <span data-ttu-id="dac42-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="dac42-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="dac42-163">Obraťte se na [tým podpory pro malé vylepšení klienta](mailto:support@small-improvements.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dac42-163">Contact [Small Improvements Client support team](mailto:support@small-improvements.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="dac42-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="dac42-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_certificate.png) 

5. <span data-ttu-id="dac42-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dac42-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dac42-168">Na hello **malé konfigurace vylepšení** klikněte na tlačítko **konfigurace malých vylepšení** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dac42-168">On hello **Small Improvements Configuration** section, click **Configure Small Improvements** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="dac42-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dac42-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_configure.png) 

7. <span data-ttu-id="dac42-171">V jiném okně prohlížeče Přihlaste se jako správce na webu společnosti tooyour malé vylepšení.</span><span class="sxs-lookup"><span data-stu-id="dac42-171">In another browser window, sign on tooyour Small Improvements company site as an administrator.</span></span>

8. <span data-ttu-id="dac42-172">Na stránce hello hlavní řídicí panel, klikněte na tlačítko **správy** tlačítko na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="dac42-172">From hello main dashboard page, click **Administration** button on hello left.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_06.png) 

9. <span data-ttu-id="dac42-174">Klikněte na tlačítko hello **jednotné přihlašování SAML** tlačítko z **integrace** části.</span><span class="sxs-lookup"><span data-stu-id="dac42-174">Click hello **SAML SSO** button from **Integrations** section.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_07.png) 

10. <span data-ttu-id="dac42-176">Na stránce instalace jednotné přihlašování hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dac42-176">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_08.png)  

    <span data-ttu-id="dac42-178">a.</span><span class="sxs-lookup"><span data-stu-id="dac42-178">a.</span></span> <span data-ttu-id="dac42-179">V hello **koncový bod HTTP** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="dac42-179">In hello **HTTP Endpoint** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="dac42-180">b.</span><span class="sxs-lookup"><span data-stu-id="dac42-180">b.</span></span> <span data-ttu-id="dac42-181">Otevřete stažený certifikát v poznámkovém bloku hello kopírování obsahu a pak ji vložit do hello **x509 certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dac42-181">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="dac42-182">c.</span><span class="sxs-lookup"><span data-stu-id="dac42-182">c.</span></span> <span data-ttu-id="dac42-183">Pokud chcete toohave jednotné přihlašování a přihlašovací formulář ověřování možnost k dispozici pro uživatele, zkontrolujte hello **povolit přístup přes heslo pro přihlášení příliš** možnost.</span><span class="sxs-lookup"><span data-stu-id="dac42-183">If you wish toohave SSO and Login form authentication option available for users, then check hello **Enable access via login/password too** option.</span></span>  

    <span data-ttu-id="dac42-184">d.</span><span class="sxs-lookup"><span data-stu-id="dac42-184">d.</span></span> <span data-ttu-id="dac42-185">Zadejte hello odpovídající hodnotu tooName hello přihlášení SSO tlačítko hello **SAML výzva** textové pole.</span><span class="sxs-lookup"><span data-stu-id="dac42-185">Enter hello appropriate value tooName hello SSO Login button in hello **SAML Prompt** textbox.</span></span>  

    <span data-ttu-id="dac42-186">e.</span><span class="sxs-lookup"><span data-stu-id="dac42-186">e.</span></span> <span data-ttu-id="dac42-187">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dac42-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="dac42-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="dac42-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="dac42-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="dac42-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="dac42-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dac42-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dac42-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dac42-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="dac42-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="dac42-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dac42-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dac42-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dac42-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dac42-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dac42-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dac42-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dac42-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="dac42-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dac42-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dac42-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dac42-203">a.</span><span class="sxs-lookup"><span data-stu-id="dac42-203">a.</span></span> <span data-ttu-id="dac42-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dac42-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dac42-205">b.</span><span class="sxs-lookup"><span data-stu-id="dac42-205">b.</span></span> <span data-ttu-id="dac42-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dac42-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dac42-207">c.</span><span class="sxs-lookup"><span data-stu-id="dac42-207">c.</span></span> <span data-ttu-id="dac42-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dac42-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="dac42-209">d.</span><span class="sxs-lookup"><span data-stu-id="dac42-209">d.</span></span> <span data-ttu-id="dac42-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dac42-210">Click **Create**.</span></span>
 
### <a name="creating-a-small-improvements-test-user"></a><span data-ttu-id="dac42-211">Vytvoření zkušebního uživatele malé vylepšení</span><span class="sxs-lookup"><span data-stu-id="dac42-211">Creating a Small Improvements test user</span></span>

<span data-ttu-id="dac42-212">Uživatelé toolog tooenable Azure AD v tooSmall vylepšení, se musí být zřízená do malých vylepšení.</span><span class="sxs-lookup"><span data-stu-id="dac42-212">tooenable Azure AD users toolog in tooSmall Improvements, they must be provisioned into Small Improvements.</span></span> <span data-ttu-id="dac42-213">V případě hello malé vylepšení zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="dac42-213">In hello case of Small Improvements, provisioning is a manual task.</span></span>

<span data-ttu-id="dac42-214">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dac42-214">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="dac42-215">Web společnosti malé vylepšení tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="dac42-215">Sign-on tooyour Small Improvements company site as an administrator.</span></span>

2. <span data-ttu-id="dac42-216">Na domovské stránce hello, přejděte na hello zbývajících toohello nabídky, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="dac42-216">From hello Home page, go toohello menu on hello left, click **Administration**.</span></span>

3. <span data-ttu-id="dac42-217">Klikněte na tlačítko hello **adresář uživatelského** tlačítko z části Správa uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dac42-217">Click hello **User Directory** button from User Management section.</span></span> 
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_10.png) 

4. <span data-ttu-id="dac42-219">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="dac42-219">Click **Add users**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_11.png) 

5. <span data-ttu-id="dac42-221">Na hello **přidat uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dac42-221">On hello **Add Users** dialog, perform hello following steps:</span></span> 

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_12.png)
    
    <span data-ttu-id="dac42-223">a.</span><span class="sxs-lookup"><span data-stu-id="dac42-223">a.</span></span> <span data-ttu-id="dac42-224">Zadejte hello **křestní jméno** uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dac42-224">Enter hello **first name** of user like **Britta**.</span></span>

    <span data-ttu-id="dac42-225">b.</span><span class="sxs-lookup"><span data-stu-id="dac42-225">b.</span></span> <span data-ttu-id="dac42-226">Zadejte hello **příjmení** uživatele jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dac42-226">Enter hello **Last name** of user like **Simon**.</span></span>

    <span data-ttu-id="dac42-227">c.</span><span class="sxs-lookup"><span data-stu-id="dac42-227">c.</span></span> <span data-ttu-id="dac42-228">Zadejte hello **e-mailu** uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="dac42-228">Enter hello **Email** of user like **brittasimon@contoso.com**.</span></span> 

    <span data-ttu-id="dac42-229">d.</span><span class="sxs-lookup"><span data-stu-id="dac42-229">d.</span></span> <span data-ttu-id="dac42-230">Můžete také tooenter hello osobní zprávu v hello **odeslat e-mailové oznámení** pole.</span><span class="sxs-lookup"><span data-stu-id="dac42-230">You can also choose tooenter hello personal message in hello **Send notification email** box.</span></span> <span data-ttu-id="dac42-231">Pokud nechcete, aby toosend hello oznámení, poté zrušte zaškrtnutí tohoto políčka.</span><span class="sxs-lookup"><span data-stu-id="dac42-231">If you do not wish toosend hello notification, then uncheck this checkbox.</span></span>

    <span data-ttu-id="dac42-232">e.</span><span class="sxs-lookup"><span data-stu-id="dac42-232">e.</span></span> <span data-ttu-id="dac42-233">Klikněte na tlačítko **vytvořte uživatele**.</span><span class="sxs-lookup"><span data-stu-id="dac42-233">Click **Create Users**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dac42-234">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="dac42-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="dac42-235">V této části povolíte tak, že udělíte přístup k vylepšení tooSmall toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dac42-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSmall Improvements.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dac42-237">**tooassign Britta Simon tooSmall vylepšení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="dac42-237">**tooassign Britta Simon tooSmall Improvements, perform hello following steps:**</span></span>

1. <span data-ttu-id="dac42-238">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dac42-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dac42-240">V seznamu aplikace hello vyberte **malé vylepšení**.</span><span class="sxs-lookup"><span data-stu-id="dac42-240">In hello applications list, select **Small Improvements**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-smallimprovements-tutorial/tutorial_smallimprovements_app.png) 

3. <span data-ttu-id="dac42-242">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dac42-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dac42-244">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dac42-244">Click **Add** button.</span></span> <span data-ttu-id="dac42-245">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dac42-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dac42-247">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="dac42-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="dac42-248">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dac42-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dac42-249">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dac42-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dac42-250">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dac42-250">Testing single sign-on</span></span>

<span data-ttu-id="dac42-251">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="dac42-251">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="dac42-252">Po kliknutí na tlačítko hello malé vylepšení dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour malé vylepšení aplikace.</span><span class="sxs-lookup"><span data-stu-id="dac42-252">When you click hello Small Improvements tile in hello Access Panel, you should get automatically signed-on tooyour Small Improvements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dac42-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dac42-253">Additional resources</span></span>

* [<span data-ttu-id="dac42-254">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dac42-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dac42-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dac42-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smallimprovements-tutorial/tutorial_general_203.png

