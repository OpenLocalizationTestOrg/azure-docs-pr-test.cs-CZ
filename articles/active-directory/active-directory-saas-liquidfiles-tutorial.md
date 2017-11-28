---
title: 'Kurz: Azure Active Directory integrace s LiquidFiles | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a LiquidFiles."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 67eb888090f81e0ceb791ed45d564b98fe1eb6d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a><span data-ttu-id="02b6a-103">Kurz: Azure Active Directory integrace s LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="02b6a-103">Tutorial: Azure Active Directory integration with LiquidFiles</span></span>

<span data-ttu-id="02b6a-104">V tomto kurzu zjistíte, jak toointegrate LiquidFiles s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="02b6a-104">In this tutorial, you learn how toointegrate LiquidFiles with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="02b6a-105">Integrace LiquidFiles s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="02b6a-105">Integrating LiquidFiles with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="02b6a-106">Můžete řídit ve službě Azure AD, který má přístup tooLiquidFiles</span><span class="sxs-lookup"><span data-stu-id="02b6a-106">You can control in Azure AD who has access tooLiquidFiles</span></span>
- <span data-ttu-id="02b6a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLiquidFiles (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="02b6a-107">You can enable your users tooautomatically get signed-on tooLiquidFiles (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="02b6a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="02b6a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="02b6a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="02b6a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="02b6a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="02b6a-110">Prerequisites</span></span>

<span data-ttu-id="02b6a-111">Integrace služby Azure AD s LiquidFiles tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="02b6a-111">tooconfigure Azure AD integration with LiquidFiles, you need hello following items:</span></span>

- <span data-ttu-id="02b6a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="02b6a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="02b6a-113">LiquidFiles jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="02b6a-113">A LiquidFiles single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="02b6a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="02b6a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="02b6a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="02b6a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="02b6a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="02b6a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="02b6a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="02b6a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="02b6a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="02b6a-118">Scenario description</span></span>
<span data-ttu-id="02b6a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="02b6a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="02b6a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="02b6a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="02b6a-121">Přidání LiquidFiles z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="02b6a-121">Adding LiquidFiles from hello gallery</span></span>
2. <span data-ttu-id="02b6a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="02b6a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-liquidfiles-from-hello-gallery"></a><span data-ttu-id="02b6a-123">Přidání LiquidFiles z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="02b6a-123">Adding LiquidFiles from hello gallery</span></span>
<span data-ttu-id="02b6a-124">tooconfigure hello integrace LiquidFiles do Azure AD, je nutné tooadd LiquidFiles hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="02b6a-124">tooconfigure hello integration of LiquidFiles into Azure AD, you need tooadd LiquidFiles from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="02b6a-125">**tooadd LiquidFiles z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02b6a-125">**tooadd LiquidFiles from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="02b6a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="02b6a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="02b6a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="02b6a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="02b6a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02b6a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="02b6a-133">Hello vyhledávacího pole zadejte **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-133">In hello search box, type **LiquidFiles**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. <span data-ttu-id="02b6a-135">Na panelu výsledků hello vyberte **LiquidFiles**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="02b6a-135">In hello results panel, select **LiquidFiles**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="02b6a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="02b6a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="02b6a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s LiquidFiles podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="02b6a-138">In this section, you configure and test Azure AD single sign-on with LiquidFiles based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="02b6a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v LiquidFiles je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="02b6a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LiquidFiles is tooa user in Azure AD.</span></span> <span data-ttu-id="02b6a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v LiquidFiles musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="02b6a-140">In other words, a link relationship between an Azure AD user and hello related user in LiquidFiles needs toobe established.</span></span>

<span data-ttu-id="02b6a-141">V LiquidFiles, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="02b6a-141">In LiquidFiles, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="02b6a-142">tooconfigure a testu Azure AD jednotné přihlašování s LiquidFiles, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="02b6a-142">tooconfigure and test Azure AD single sign-on with LiquidFiles, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="02b6a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="02b6a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="02b6a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="02b6a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="02b6a-145">**[Vytvoření zkušebního uživatele LiquidFiles](#creating-a-liquidfiles-test-user)**  -toohave protějšek Britta Simon v LiquidFiles, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="02b6a-145">**[Creating a LiquidFiles test user](#creating-a-liquidfiles-test-user)** - toohave a counterpart of Britta Simon in LiquidFiles that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="02b6a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02b6a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="02b6a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="02b6a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="02b6a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="02b6a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="02b6a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="02b6a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LiquidFiles application.</span></span>

<span data-ttu-id="02b6a-150">**tooconfigure Azure AD jednotné přihlašování s LiquidFiles, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02b6a-150">**tooconfigure Azure AD single sign-on with LiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="02b6a-151">V portálu Azure, na hello hello **LiquidFiles** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-151">In hello Azure portal, on hello **LiquidFiles** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="02b6a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02b6a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. <span data-ttu-id="02b6a-155">Na hello **LiquidFiles domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="02b6a-155">On hello **LiquidFiles Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    <span data-ttu-id="02b6a-157">a.</span><span class="sxs-lookup"><span data-stu-id="02b6a-157">a.</span></span> <span data-ttu-id="02b6a-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>/saml/init`</span><span class="sxs-lookup"><span data-stu-id="02b6a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/init`</span></span>

    <span data-ttu-id="02b6a-159">b.</span><span class="sxs-lookup"><span data-stu-id="02b6a-159">b.</span></span> <span data-ttu-id="02b6a-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>`</span><span class="sxs-lookup"><span data-stu-id="02b6a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>`</span></span>

    <span data-ttu-id="02b6a-161">c.</span><span class="sxs-lookup"><span data-stu-id="02b6a-161">c.</span></span> <span data-ttu-id="02b6a-162">b.</span><span class="sxs-lookup"><span data-stu-id="02b6a-162">b.</span></span> <span data-ttu-id="02b6a-163">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<YOUR_SERVER_URL>/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="02b6a-163">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="02b6a-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="02b6a-164">These values are not real.</span></span> <span data-ttu-id="02b6a-165">Aktualizace tyto hodnoty s hello skutečná adresa URL přihlašování, identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="02b6a-165">Update these values with hello actual Sign-On URL, Identifier and, Reply URL.</span></span> <span data-ttu-id="02b6a-166">Obraťte se na [tým podpory LiquidFiles klienta](https://www.liquidfiles.com/support.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="02b6a-166">Contact [LiquidFiles Client support team](https://www.liquidfiles.com/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="02b6a-167">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="02b6a-167">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. <span data-ttu-id="02b6a-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02b6a-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="02b6a-171">Na hello **LiquidFiles konfigurace** klikněte na tlačítko **konfigurace LiquidFiles** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="02b6a-171">On hello **LiquidFiles Configuration** section, click **Configure LiquidFiles** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="02b6a-172">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="02b6a-172">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. <span data-ttu-id="02b6a-174">Web společnosti LiquidFiles tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="02b6a-174">Sign-on tooyour LiquidFiles company site as administrator.</span></span>

8. <span data-ttu-id="02b6a-175">Klikněte na tlačítko **jednotné přihlašování** v hello **správce > Konfigurace** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="02b6a-175">Click **Single Sign-On** in hello **Admin > Configuration** from hello menu.</span></span>

9. <span data-ttu-id="02b6a-176">Na hello **konfigurace přihlášení** proveďte následující kroky hello</span><span class="sxs-lookup"><span data-stu-id="02b6a-176">On hello **Single Sign-On Configuration** page, perform hello following steps</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    <span data-ttu-id="02b6a-178">a.</span><span class="sxs-lookup"><span data-stu-id="02b6a-178">a.</span></span> <span data-ttu-id="02b6a-179">Jako **jedné přihlašovací na metody**, vyberte **SAML 2**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-179">As **Single Sign On Method**, select **SAML 2**.</span></span>

    <span data-ttu-id="02b6a-180">b.</span><span class="sxs-lookup"><span data-stu-id="02b6a-180">b.</span></span> <span data-ttu-id="02b6a-181">V hello **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="02b6a-181">In hello **IDP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="02b6a-182">c.</span><span class="sxs-lookup"><span data-stu-id="02b6a-182">c.</span></span> <span data-ttu-id="02b6a-183">V hello **adresy URL odhlašovací IDP** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="02b6a-183">In hello **IDP Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="02b6a-184">d.</span><span class="sxs-lookup"><span data-stu-id="02b6a-184">d.</span></span> <span data-ttu-id="02b6a-185">V hello **otisků certifikátu IDP** textovému poli, vložte hello **kryptografický OTISK** hodnotu, která jste zkopírovali z portálu Azure...</span><span class="sxs-lookup"><span data-stu-id="02b6a-185">In hello **IDP Cert Fingerprint** textbox, paste hello **THUMBPRINT** value which you have copied from Azure portal..</span></span>

    <span data-ttu-id="02b6a-186">e.</span><span class="sxs-lookup"><span data-stu-id="02b6a-186">e.</span></span> <span data-ttu-id="02b6a-187">V textovém poli Formát názvu identifikátor hello, zadejte hodnotu hello **urn: oasis: názvy: tc: SAML:1.1:nameid-formátu: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-187">In hello Name Identifier Format textbox, type hello value **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="02b6a-188">f.</span><span class="sxs-lookup"><span data-stu-id="02b6a-188">f.</span></span> <span data-ttu-id="02b6a-189">V hello ověřovacího kontextu textového pole zadejte hodnotu hello **urn: oasis: názvy: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-189">In hello Authn Context textbox, type hello value **urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport**.</span></span>

    <span data-ttu-id="02b6a-190">g.</span><span class="sxs-lookup"><span data-stu-id="02b6a-190">g.</span></span> <span data-ttu-id="02b6a-191">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-191">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="02b6a-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="02b6a-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="02b6a-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="02b6a-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="02b6a-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="02b6a-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="02b6a-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="02b6a-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="02b6a-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="02b6a-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="02b6a-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02b6a-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="02b6a-199">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="02b6a-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="02b6a-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="02b6a-203">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="02b6a-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="02b6a-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="02b6a-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="02b6a-207">a.</span><span class="sxs-lookup"><span data-stu-id="02b6a-207">a.</span></span> <span data-ttu-id="02b6a-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="02b6a-209">b.</span><span class="sxs-lookup"><span data-stu-id="02b6a-209">b.</span></span> <span data-ttu-id="02b6a-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="02b6a-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="02b6a-211">c.</span><span class="sxs-lookup"><span data-stu-id="02b6a-211">c.</span></span> <span data-ttu-id="02b6a-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="02b6a-213">d.</span><span class="sxs-lookup"><span data-stu-id="02b6a-213">d.</span></span> <span data-ttu-id="02b6a-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-214">Click **Create**.</span></span>
 
### <a name="creating-a-liquidfiles-test-user"></a><span data-ttu-id="02b6a-215">Vytvoření zkušebního uživatele LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="02b6a-215">Creating a LiquidFiles test user</span></span>

<span data-ttu-id="02b6a-216">Hello cílem této části je toocreate volal Britta Simon v LiquidFiles uživatele.</span><span class="sxs-lookup"><span data-stu-id="02b6a-216">hello objective of this section is toocreate a user called Britta Simon in LiquidFiles.</span></span> <span data-ttu-id="02b6a-217">Spolupracovat s vaší tooget správce serveru LiquidFiles sami přidána jako uživatel před protokolování v tooyour LiquidFiles aplikace.</span><span class="sxs-lookup"><span data-stu-id="02b6a-217">Work with your LiquidFiles server administrator tooget yourself added as a user before logging in tooyour LiquidFiles application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="02b6a-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="02b6a-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="02b6a-219">V této části povolíte tak, že udělíte přístup tooLiquidFiles toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="02b6a-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLiquidFiles.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="02b6a-221">**tooassign Britta Simon tooLiquidFiles, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="02b6a-221">**tooassign Britta Simon tooLiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="02b6a-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="02b6a-224">V seznamu aplikace hello vyberte **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-224">In hello applications list, select **LiquidFiles**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. <span data-ttu-id="02b6a-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="02b6a-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="02b6a-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="02b6a-228">Click **Add** button.</span></span> <span data-ttu-id="02b6a-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02b6a-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="02b6a-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="02b6a-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="02b6a-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02b6a-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="02b6a-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="02b6a-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="02b6a-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="02b6a-234">Testing single sign-on</span></span>

<span data-ttu-id="02b6a-235">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="02b6a-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="02b6a-236">Po kliknutí na tlačítko hello LiquidFiles dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour LiquidFiles aplikace.</span><span class="sxs-lookup"><span data-stu-id="02b6a-236">When you click hello LiquidFiles tile in hello Access Panel, you should get automatically signed-on tooyour LiquidFiles application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02b6a-237">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="02b6a-237">Additional resources</span></span>

* [<span data-ttu-id="02b6a-238">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="02b6a-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="02b6a-239">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="02b6a-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

