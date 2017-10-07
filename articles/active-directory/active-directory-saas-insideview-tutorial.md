---
title: 'Kurz: Azure Active Directory integrace s InsideView | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a InsideView."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 979c0c24f3a18a193616061b8c2e78292233a56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a><span data-ttu-id="4fc75-103">Kurz: Azure Active Directory integrace s InsideView</span><span class="sxs-lookup"><span data-stu-id="4fc75-103">Tutorial: Azure Active Directory integration with InsideView</span></span>

<span data-ttu-id="4fc75-104">V tomto kurzu zjistíte, jak toointegrate InsideView s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4fc75-104">In this tutorial, you learn how toointegrate InsideView with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4fc75-105">Integrace InsideView s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4fc75-105">Integrating InsideView with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4fc75-106">Můžete řídit ve službě Azure AD, který má přístup tooInsideView</span><span class="sxs-lookup"><span data-stu-id="4fc75-106">You can control in Azure AD who has access tooInsideView</span></span>
- <span data-ttu-id="4fc75-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooInsideView (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fc75-107">You can enable your users tooautomatically get signed-on tooInsideView (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4fc75-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4fc75-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4fc75-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4fc75-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4fc75-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4fc75-110">Prerequisites</span></span>

<span data-ttu-id="4fc75-111">Integrace služby Azure AD s InsideView tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4fc75-111">tooconfigure Azure AD integration with InsideView, you need hello following items:</span></span>

- <span data-ttu-id="4fc75-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fc75-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4fc75-113">InsideView jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4fc75-113">A InsideView single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4fc75-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4fc75-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4fc75-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4fc75-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4fc75-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4fc75-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4fc75-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4fc75-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4fc75-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4fc75-118">Scenario description</span></span>
<span data-ttu-id="4fc75-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4fc75-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4fc75-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4fc75-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4fc75-121">Přidání InsideView z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4fc75-121">Adding InsideView from hello gallery</span></span>
2. <span data-ttu-id="4fc75-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fc75-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insideview-from-hello-gallery"></a><span data-ttu-id="4fc75-123">Přidání InsideView z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4fc75-123">Adding InsideView from hello gallery</span></span>
<span data-ttu-id="4fc75-124">tooconfigure hello integrace InsideView v tooAzure AD, je nutné tooadd InsideView hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4fc75-124">tooconfigure hello integration of InsideView in tooAzure AD, you need tooadd InsideView from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4fc75-125">**tooadd InsideView z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fc75-125">**tooadd InsideView from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fc75-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4fc75-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4fc75-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4fc75-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4fc75-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fc75-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4fc75-133">Hello vyhledávacího pole zadejte **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-133">In hello search box, type **InsideView**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. <span data-ttu-id="4fc75-135">Na panelu výsledků hello vyberte **InsideView**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fc75-135">In hello results panel, select **InsideView**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4fc75-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fc75-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4fc75-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s InsideView podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="4fc75-138">In this section, you configure and test Azure AD single sign-on with InsideView based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4fc75-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v InsideView je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fc75-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in InsideView is tooa user in Azure AD.</span></span> <span data-ttu-id="4fc75-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v InsideView musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="4fc75-140">In other words, a link relationship between an Azure AD user and hello related user in InsideView needs toobe established.</span></span>

<span data-ttu-id="4fc75-141">V InsideView, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="4fc75-141">In InsideView, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4fc75-142">tooconfigure a testu Azure AD jednotné přihlašování s InsideView, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="4fc75-142">tooconfigure and test Azure AD single sign-on with InsideView, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4fc75-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4fc75-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4fc75-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4fc75-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4fc75-145">**[Vytvoření zkušebního uživatele InsideView](#creating-a-insideview-test-user)**  -toohave protějšek Britta Simon v InsideView, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4fc75-145">**[Creating a InsideView test user](#creating-a-insideview-test-user)** - toohave a counterpart of Britta Simon in InsideView that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4fc75-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fc75-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4fc75-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="4fc75-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4fc75-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fc75-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4fc75-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci InsideView.</span><span class="sxs-lookup"><span data-stu-id="4fc75-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your InsideView application.</span></span>

<span data-ttu-id="4fc75-150">**tooconfigure Azure AD jednotné přihlašování s InsideView, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fc75-150">**tooconfigure Azure AD single sign-on with InsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fc75-151">V portálu Azure, na hello hello **InsideView** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-151">In hello Azure portal, on hello **InsideView** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4fc75-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fc75-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. <span data-ttu-id="4fc75-155">Na hello **InsideView domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4fc75-155">On hello **InsideView Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    <span data-ttu-id="4fc75-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://my.insideview.com/iv/<STS Name>/login.iv`</span><span class="sxs-lookup"><span data-stu-id="4fc75-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://my.insideview.com/iv/<STS Name>/login.iv`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4fc75-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="4fc75-158">This value is not real.</span></span> <span data-ttu-id="4fc75-159">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4fc75-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="4fc75-160">Obraťte se na [tým podpory InsideView ](mailto:support@insideview.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4fc75-160">Contact [InsideView support team ](mailto:support@insideview.com) tooget this value.</span></span>
 
4. <span data-ttu-id="4fc75-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4fc75-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. <span data-ttu-id="4fc75-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fc75-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4fc75-165">Na hello **InsideView konfigurace** klikněte na tlačítko **konfigurace InsideView** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="4fc75-165">On hello **InsideView Configuration** section, click **Configure InsideView** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4fc75-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="4fc75-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. <span data-ttu-id="4fc75-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti InsideView tooyour.</span><span class="sxs-lookup"><span data-stu-id="4fc75-168">In a different web browser window, log in tooyour InsideView company site as an administrator.</span></span>

8. <span data-ttu-id="4fc75-169">V panelu nástrojů hello hello nahoře, klikněte na **správce**, **SingleSignOn nastavení**a potom klikněte na **přidat SAML**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-169">In hello toolbar on hello top, click **Admin**, **SingleSignOn Settings**, and then click **Add SAML**.</span></span>
   
   <span data-ttu-id="4fc75-170">![Jednotné přihlašování SAML nastavení](./media/active-directory-saas-insideview-tutorial/ic794135.png "jednotného přihlašování SAML nastavení")</span><span class="sxs-lookup"><span data-stu-id="4fc75-170">![SAML Single Sign On Settings](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML Single Sign On Settings")</span></span>

9. <span data-ttu-id="4fc75-171">V hello **přidat nové SAML** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4fc75-171">In hello **Add a New SAML** section, perform hello following steps:</span></span>

    <span data-ttu-id="4fc75-172">![Přidat nové SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "přidat nové SAML")</span><span class="sxs-lookup"><span data-stu-id="4fc75-172">![Add a New SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "Add a New SAML")</span></span>
   
    <span data-ttu-id="4fc75-173">a.</span><span class="sxs-lookup"><span data-stu-id="4fc75-173">a.</span></span> <span data-ttu-id="4fc75-174">V hello **název služby tokenů zabezpečení** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="4fc75-174">In hello **STS Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="4fc75-175">b.</span><span class="sxs-lookup"><span data-stu-id="4fc75-175">b.</span></span> <span data-ttu-id="4fc75-176">V **nevyžádané koncový bod SamlP/WS-Fed** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc75-176">In **SamlP/WS-Fed Unsolicited EndPoint** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="4fc75-177">c.</span><span class="sxs-lookup"><span data-stu-id="4fc75-177">c.</span></span> <span data-ttu-id="4fc75-178">Otevření kódování base-64 kódovaného certifikátu, který jste si stáhli z Azure portálu kopie hello obsahu ho do schránky, a pak ji vložit toohello **certifikát služby tokenů zabezpečení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="4fc75-178">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **STS Certificate** textbox.</span></span>

    <span data-ttu-id="4fc75-179">d.</span><span class="sxs-lookup"><span data-stu-id="4fc75-179">d.</span></span> <span data-ttu-id="4fc75-180">V hello **mapování Id uživatele Crm** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="4fc75-180">In hello **Crm User Id Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
        
    <span data-ttu-id="4fc75-181">e.</span><span class="sxs-lookup"><span data-stu-id="4fc75-181">e.</span></span> <span data-ttu-id="4fc75-182">V hello **Crm e-mailu mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="4fc75-182">In hello **Crm Email Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="4fc75-183">f.</span><span class="sxs-lookup"><span data-stu-id="4fc75-183">f.</span></span> <span data-ttu-id="4fc75-184">V hello **Crm křestní jméno mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="4fc75-184">In hello **Crm First Name Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="4fc75-185">g.</span><span class="sxs-lookup"><span data-stu-id="4fc75-185">g.</span></span> <span data-ttu-id="4fc75-186">V hello **Crm lastName mapování** textovému poli, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="4fc75-186">In hello **Crm lastName Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>  

    <span data-ttu-id="4fc75-187">h.</span><span class="sxs-lookup"><span data-stu-id="4fc75-187">h.</span></span> <span data-ttu-id="4fc75-188">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="4fc75-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="4fc75-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4fc75-190">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="4fc75-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4fc75-191">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4fc75-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4fc75-192">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fc75-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="4fc75-193">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="4fc75-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4fc75-195">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fc75-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fc75-196">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4fc75-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4fc75-198">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4fc75-200">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="4fc75-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4fc75-202">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4fc75-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4fc75-204">a.</span><span class="sxs-lookup"><span data-stu-id="4fc75-204">a.</span></span> <span data-ttu-id="4fc75-205">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4fc75-206">b.</span><span class="sxs-lookup"><span data-stu-id="4fc75-206">b.</span></span> <span data-ttu-id="4fc75-207">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4fc75-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4fc75-208">c.</span><span class="sxs-lookup"><span data-stu-id="4fc75-208">c.</span></span> <span data-ttu-id="4fc75-209">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4fc75-210">d.</span><span class="sxs-lookup"><span data-stu-id="4fc75-210">d.</span></span> <span data-ttu-id="4fc75-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-211">Click **Create**.</span></span>
 
### <a name="creating-a-insideview-test-user"></a><span data-ttu-id="4fc75-212">Vytvoření zkušebního uživatele InsideView</span><span class="sxs-lookup"><span data-stu-id="4fc75-212">Creating a InsideView test user</span></span>

<span data-ttu-id="4fc75-213">Uživatelé toolog tooenable Azure AD v tooInsideView, se musí být zřízená v tooInsideView.</span><span class="sxs-lookup"><span data-stu-id="4fc75-213">tooenable Azure AD users toolog in tooInsideView, they must be provisioned in tooInsideView.</span></span> <span data-ttu-id="4fc75-214">V případě hello InsideView zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="4fc75-214">In hello case of InsideView, provisioning is a manual task.</span></span>

<span data-ttu-id="4fc75-215">Uživatelé tooget nebo kontakty, které jsou vytvořené v InsideView, obraťte se na [tým podpory InsideView](mailto:support@insideview.com).</span><span class="sxs-lookup"><span data-stu-id="4fc75-215">tooget users or contacts created in InsideView, Contact [InsideView support team](mailto:support@insideview.com).</span></span>

>[!NOTE]
><span data-ttu-id="4fc75-216">Můžete použít všechny ostatní InsideView uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované InsideView tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4fc75-216">You can use any other InsideView user account creation tools or APIs provided by InsideView tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4fc75-217">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="4fc75-217">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4fc75-218">V této části povolíte tak, že udělíte přístup tooInsideView toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4fc75-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInsideView.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4fc75-220">**tooassign Britta Simon tooInsideView, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4fc75-220">**tooassign Britta Simon tooInsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="4fc75-221">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4fc75-223">V seznamu aplikace hello vyberte **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-223">In hello applications list, select **InsideView**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. <span data-ttu-id="4fc75-225">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4fc75-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4fc75-227">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4fc75-227">Click **Add** button.</span></span> <span data-ttu-id="4fc75-228">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc75-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4fc75-230">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="4fc75-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4fc75-231">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc75-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4fc75-232">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc75-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4fc75-233">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4fc75-233">Testing single sign-on</span></span>

<span data-ttu-id="4fc75-234">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4fc75-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4fc75-235">Když kliknete na dlaždici InsideView hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour InsideView aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fc75-235">When you click hello InsideView tile in hello Access Panel, you should get automatically signed-on tooyour InsideView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fc75-236">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4fc75-236">Additional resources</span></span>

* [<span data-ttu-id="4fc75-237">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4fc75-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4fc75-238">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4fc75-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

