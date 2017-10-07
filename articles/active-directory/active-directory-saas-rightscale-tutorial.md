---
title: 'Kurz: Azure Active Directory integrace s Rightscale | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Rightscale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 53b927804a1e0f895778a164386459a4ea816f98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a><span data-ttu-id="5f366-103">Kurz: Azure Active Directory integrace s Rightscale</span><span class="sxs-lookup"><span data-stu-id="5f366-103">Tutorial: Azure Active Directory integration with Rightscale</span></span>

<span data-ttu-id="5f366-104">V tomto kurzu zjistíte, jak toointegrate Rightscale s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5f366-104">In this tutorial, you learn how toointegrate Rightscale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5f366-105">Integrace Rightscale s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5f366-105">Integrating Rightscale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5f366-106">Můžete řídit ve službě Azure AD, který má přístup tooRightscale</span><span class="sxs-lookup"><span data-stu-id="5f366-106">You can control in Azure AD who has access tooRightscale</span></span>
- <span data-ttu-id="5f366-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooRightscale (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f366-107">You can enable your users tooautomatically get signed-on tooRightscale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5f366-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f366-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5f366-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5f366-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f366-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f366-110">Prerequisites</span></span>

<span data-ttu-id="5f366-111">Integrace služby Azure AD s Rightscale tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5f366-111">tooconfigure Azure AD integration with Rightscale, you need hello following items:</span></span>

- <span data-ttu-id="5f366-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f366-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5f366-113">Rightscale jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5f366-113">A Rightscale single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5f366-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f366-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5f366-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5f366-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5f366-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5f366-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5f366-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f366-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5f366-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5f366-118">Scenario description</span></span>
<span data-ttu-id="5f366-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f366-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5f366-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5f366-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5f366-121">Přidání Rightscale z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f366-121">Adding Rightscale from hello gallery</span></span>
2. <span data-ttu-id="5f366-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f366-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightscale-from-hello-gallery"></a><span data-ttu-id="5f366-123">Přidání Rightscale z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f366-123">Adding Rightscale from hello gallery</span></span>
<span data-ttu-id="5f366-124">tooconfigure hello integrace Rightscale do Azure AD, je nutné tooadd Rightscale hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5f366-124">tooconfigure hello integration of Rightscale into Azure AD, you need tooadd Rightscale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5f366-125">**tooadd Rightscale z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f366-125">**tooadd Rightscale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f366-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f366-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5f366-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5f366-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5f366-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f366-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5f366-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f366-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5f366-133">Hello vyhledávacího pole zadejte **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5f366-133">In hello search box, type **Rightscale**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. <span data-ttu-id="5f366-135">Na panelu výsledků hello vyberte **Rightscale**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f366-135">In hello results panel, select **Rightscale**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5f366-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f366-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5f366-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Rightscale podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5f366-138">In this section, you configure and test Azure AD single sign-on with Rightscale based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5f366-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Rightscale je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f366-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rightscale is tooa user in Azure AD.</span></span> <span data-ttu-id="5f366-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Rightscale musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5f366-140">In other words, a link relationship between an Azure AD user and hello related user in Rightscale needs toobe established.</span></span>

<span data-ttu-id="5f366-141">V Rightscale, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5f366-141">In Rightscale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5f366-142">tooconfigure a testu Azure AD jednotné přihlašování s Rightscale, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5f366-142">tooconfigure and test Azure AD single sign-on with Rightscale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5f366-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5f366-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5f366-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f366-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5f366-145">**[Vytvoření zkušebního uživatele Rightscale](#creating-a-rightscale-test-user)**  -toohave protějšek Britta Simon v Rightscale, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f366-145">**[Creating a Rightscale test user](#creating-a-rightscale-test-user)** - toohave a counterpart of Britta Simon in Rightscale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5f366-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f366-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5f366-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5f366-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5f366-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f366-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5f366-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Rightscale.</span><span class="sxs-lookup"><span data-stu-id="5f366-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rightscale application.</span></span>

<span data-ttu-id="5f366-150">**tooconfigure Azure AD jednotné přihlašování s Rightscale, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f366-150">**tooconfigure Azure AD single sign-on with Rightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f366-151">V portálu Azure, na hello hello **Rightscale** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5f366-151">In hello Azure portal, on hello **Rightscale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5f366-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f366-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. <span data-ttu-id="5f366-155">Na hello **Rightscale domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu** nemáte tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="5f366-155">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode** you do not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. <span data-ttu-id="5f366-157">Na hello **Rightscale domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5f366-157">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    <span data-ttu-id="5f366-159">a.</span><span class="sxs-lookup"><span data-stu-id="5f366-159">a.</span></span> <span data-ttu-id="5f366-160">Klikněte na hello **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="5f366-160">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="5f366-161">b.</span><span class="sxs-lookup"><span data-stu-id="5f366-161">b.</span></span> <span data-ttu-id="5f366-162">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://login.rightscale.com/`</span><span class="sxs-lookup"><span data-stu-id="5f366-162">In hello **Sign On URL** textbox, type hello URL: `https://login.rightscale.com/`</span></span>

5. <span data-ttu-id="5f366-163">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5f366-163">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. <span data-ttu-id="5f366-165">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f366-165">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5f366-167">Na hello **Rightscale konfigurace** klikněte na tlačítko **konfigurace Rightscale** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5f366-167">On hello **Rightscale Configuration** section, click **Configure Rightscale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5f366-168">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5f366-168">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="5f366-169">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="5f366-169">![Configure Single Sign-On](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span></span>
8. <span data-ttu-id="5f366-170">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, musíte klienta RightScale toosign na tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="5f366-170">tooget SSO configured for your application, you need toosign-on tooyour RightScale tenant as an administrator.</span></span>

    <span data-ttu-id="5f366-171">a.</span><span class="sxs-lookup"><span data-stu-id="5f366-171">a.</span></span> <span data-ttu-id="5f366-172">V nabídce hello hello nahoře, klikněte na tlačítko hello **nastavení** a vyberte **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5f366-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    <span data-ttu-id="5f366-174">b.</span><span class="sxs-lookup"><span data-stu-id="5f366-174">b.</span></span> <span data-ttu-id="5f366-175">Klikněte na tlačítko hello "**nové**" tlačítko tooadd **vaše zprostředkovatelů Identity SAML**.</span><span class="sxs-lookup"><span data-stu-id="5f366-175">Click hello "**new**" button tooadd **Your SAML Identity Providers**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    <span data-ttu-id="5f366-177">c.</span><span class="sxs-lookup"><span data-stu-id="5f366-177">c.</span></span> <span data-ttu-id="5f366-178">V textovém poli hello z **zobrazovaný název**, zadejte název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="5f366-178">In hello textbox of **Display Name**, input your company name.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    <span data-ttu-id="5f366-180">d.</span><span class="sxs-lookup"><span data-stu-id="5f366-180">d.</span></span> <span data-ttu-id="5f366-181">Vyberte **spouštěná RightScale povolit jednotné přihlašování pomocí zjišťování nápovědu** a vstupní vaše **název domény** v hello pod textové pole.</span><span class="sxs-lookup"><span data-stu-id="5f366-181">Select **Allow RightScale-initiated SSO using a discovery hint** and input your **domain name** in hello below textbox.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    <span data-ttu-id="5f366-183">e.</span><span class="sxs-lookup"><span data-stu-id="5f366-183">e.</span></span> <span data-ttu-id="5f366-184">Vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure do **koncového bodu jednotného přihlašování SAML** v RightScale.</span><span class="sxs-lookup"><span data-stu-id="5f366-184">Paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal into **SAML SSO Endpoint** in RightScale.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    <span data-ttu-id="5f366-186">f.</span><span class="sxs-lookup"><span data-stu-id="5f366-186">f.</span></span> <span data-ttu-id="5f366-187">Vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure do **SAML EntityID** v RightScale.</span><span class="sxs-lookup"><span data-stu-id="5f366-187">Paste hello value of **SAML Entity ID** which you have copied from Azure portal into **SAML EntityID** in RightScale.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    <span data-ttu-id="5f366-189">g.</span><span class="sxs-lookup"><span data-stu-id="5f366-189">g.</span></span> <span data-ttu-id="5f366-190">Klikněte na tlačítko **prohlížeče** tlačítko tooupload hello certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f366-190">Click **Browser** button tooupload hello certificate which you downloaded from Azure portal.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    <span data-ttu-id="5f366-192">h.</span><span class="sxs-lookup"><span data-stu-id="5f366-192">h.</span></span> <span data-ttu-id="5f366-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5f366-193">Click **Save**.</span></span>
<CE>
> [!TIP]
> <span data-ttu-id="5f366-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5f366-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5f366-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5f366-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5f366-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5f366-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5f366-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f366-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="5f366-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5f366-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5f366-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f366-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f366-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f366-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5f366-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5f366-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5f366-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5f366-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5f366-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f366-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5f366-209">a.</span><span class="sxs-lookup"><span data-stu-id="5f366-209">a.</span></span> <span data-ttu-id="5f366-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5f366-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5f366-211">b.</span><span class="sxs-lookup"><span data-stu-id="5f366-211">b.</span></span> <span data-ttu-id="5f366-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5f366-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5f366-213">c.</span><span class="sxs-lookup"><span data-stu-id="5f366-213">c.</span></span> <span data-ttu-id="5f366-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5f366-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5f366-215">d.</span><span class="sxs-lookup"><span data-stu-id="5f366-215">d.</span></span> <span data-ttu-id="5f366-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5f366-216">Click **Create**.</span></span>
 
### <a name="creating-a-rightscale-test-user"></a><span data-ttu-id="5f366-217">Vytvoření zkušebního uživatele Rightscale</span><span class="sxs-lookup"><span data-stu-id="5f366-217">Creating a Rightscale test user</span></span>

<span data-ttu-id="5f366-218">V této části vytvoříte volal Britta Simon v RightScale uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f366-218">In this section, you create a user called Britta Simon in RightScale.</span></span> <span data-ttu-id="5f366-219">Práce s [tým podpory Rightscale klienta](mailto:support@rightscale.com) tooadd hello uživatelé v platformě RightScale hello.</span><span class="sxs-lookup"><span data-stu-id="5f366-219">Work with [Rightscale Client support team](mailto:support@rightscale.com) tooadd hello users in hello RightScale platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5f366-220">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5f366-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5f366-221">V této části povolíte tak, že udělíte přístup tooRightscale toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f366-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightscale.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5f366-223">**tooassign Britta Simon tooRightscale, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f366-223">**tooassign Britta Simon tooRightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f366-224">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f366-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5f366-226">V seznamu aplikace hello vyberte **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5f366-226">In hello applications list, select **Rightscale**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. <span data-ttu-id="5f366-228">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5f366-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5f366-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f366-230">Click **Add** button.</span></span> <span data-ttu-id="5f366-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f366-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5f366-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5f366-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5f366-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f366-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5f366-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f366-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5f366-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f366-236">Testing single sign-on</span></span>

<span data-ttu-id="5f366-237">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="5f366-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="5f366-238">Když kliknete na dlaždici RightScale hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour RightScale aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f366-238">When you click hello RightScale tile in hello Access Panel, you should get automatically signed-on tooyour RightScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f366-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5f366-239">Additional resources</span></span>

* [<span data-ttu-id="5f366-240">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f366-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5f366-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5f366-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

