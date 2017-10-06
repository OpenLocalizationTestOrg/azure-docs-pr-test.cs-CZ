---
title: 'Kurz: Azure Active Directory integrace s Hackerone | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Hackerone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: c9dc033e26e79a7233dcfb3899c62684d4a19652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="a0eef-103">Kurz: Azure Active Directory integrace s HackerOne</span><span class="sxs-lookup"><span data-stu-id="a0eef-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="a0eef-104">V tomto kurzu zjistíte, jak toointegrate HackerOne s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a0eef-104">In this tutorial, you learn how toointegrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a0eef-105">Integrace HackerOne s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a0eef-105">Integrating HackerOne with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a0eef-106">Můžete řídit ve službě Azure AD, který má přístup tooHackerOne</span><span class="sxs-lookup"><span data-stu-id="a0eef-106">You can control in Azure AD who has access tooHackerOne</span></span>
- <span data-ttu-id="a0eef-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooHackerOne (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0eef-107">You can enable your users tooautomatically get signed-on tooHackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a0eef-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a0eef-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a0eef-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a0eef-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0eef-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a0eef-110">Prerequisites</span></span>

<span data-ttu-id="a0eef-111">Integrace služby Azure AD s HackerOne tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a0eef-111">tooconfigure Azure AD integration with HackerOne, you need hello following items:</span></span>

- <span data-ttu-id="a0eef-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0eef-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a0eef-113">HackerOne jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a0eef-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a0eef-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0eef-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a0eef-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a0eef-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a0eef-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a0eef-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a0eef-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a0eef-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a0eef-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a0eef-118">Scenario description</span></span>
<span data-ttu-id="a0eef-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0eef-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a0eef-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a0eef-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a0eef-121">Přidání HackerOne z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a0eef-121">Adding HackerOne from hello gallery</span></span>
2. <span data-ttu-id="a0eef-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a0eef-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-hello-gallery"></a><span data-ttu-id="a0eef-123">Přidání HackerOne z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a0eef-123">Adding HackerOne from hello gallery</span></span>
<span data-ttu-id="a0eef-124">tooconfigure hello integrace HackerOne do Azure AD, je nutné tooadd HackerOne hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a0eef-124">tooconfigure hello integration of HackerOne into Azure AD, you need tooadd HackerOne from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a0eef-125">**tooadd HackerOne z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a0eef-125">**tooadd HackerOne from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0eef-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a0eef-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a0eef-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a0eef-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a0eef-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a0eef-133">Hello vyhledávacího pole zadejte **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-133">In hello search box, type **HackerOne**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="a0eef-135">Na panelu výsledků hello vyberte **HackerOne**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0eef-135">In hello results panel, select **HackerOne**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a0eef-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a0eef-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="a0eef-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HackerOne podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a0eef-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a0eef-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v HackerOne je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a0eef-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in HackerOne is tooa user in Azure AD.</span></span> <span data-ttu-id="a0eef-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v HackerOne musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a0eef-140">In other words, a link relationship between an Azure AD user and hello related user in HackerOne needs toobe established.</span></span>

<span data-ttu-id="a0eef-141">V HackerOne, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="a0eef-141">In HackerOne, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a0eef-142">tooconfigure a testu Azure AD jednotné přihlašování s HackerOne, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a0eef-142">tooconfigure and test Azure AD single sign-on with HackerOne, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a0eef-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a0eef-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a0eef-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0eef-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a0eef-145">**[Vytvoření zkušebního uživatele HackerOne](#creating-a-hackerone-test-user)**  -toohave protějšek Britta Simon v HackerOne, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0eef-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - toohave a counterpart of Britta Simon in HackerOne that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a0eef-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a0eef-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a0eef-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a0eef-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a0eef-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a0eef-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a0eef-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci HackerOne.</span><span class="sxs-lookup"><span data-stu-id="a0eef-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="a0eef-150">**tooconfigure Azure AD jednotné přihlašování s HackerOne, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a0eef-150">**tooconfigure Azure AD single sign-on with HackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0eef-151">V portálu Azure, na hello hello **HackerOne** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-151">In hello Azure portal, on hello **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a0eef-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a0eef-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="a0eef-155">Na hello **HackerOne jedním přihlašovací adresa URL a identifikátor** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a0eef-155">On hello **HackerOne Single sign-on URL and Identifier** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="a0eef-157">a.</span><span class="sxs-lookup"><span data-stu-id="a0eef-157">a.</span></span> <span data-ttu-id="a0eef-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="a0eef-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="a0eef-159">b.</span><span class="sxs-lookup"><span data-stu-id="a0eef-159">b.</span></span> <span data-ttu-id="a0eef-160">V hello **identifikátor** textovému poli, zadejte adresu URL jako:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="a0eef-160">In hello **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="a0eef-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a0eef-161">This value is not real.</span></span> <span data-ttu-id="a0eef-162">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a0eef-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a0eef-163">Obraťte se na [tým podpory HackerOne](mailto:support@hackerone.com) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-163">Contact [HackerOne support team](mailto:support@hackerone.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="a0eef-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a0eef-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="a0eef-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a0eef-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a0eef-168">Na hello **HackerOne konfigurace** klikněte na tlačítko **konfigurace HackerOne** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a0eef-168">On hello **HackerOne Configuration** section, click **Configure HackerOne** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a0eef-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a0eef-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="a0eef-171">Přihlašování tooyour HackerOne klienta jako správce.</span><span class="sxs-lookup"><span data-stu-id="a0eef-171">Sign On tooyour HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="a0eef-172">V nabídce hello hello nahoře, klikněte na tlačítko hello "**nastavení**."</span><span class="sxs-lookup"><span data-stu-id="a0eef-172">In hello menu on hello top, click hello "**Settings**."</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="a0eef-174">Přejděte příliš"**ověřování**"a klikněte na tlačítko"**přidejte nastavení SAML**."</span><span class="sxs-lookup"><span data-stu-id="a0eef-174">Navigate too"**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="a0eef-176">Na hello **SAML nastavení** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a0eef-176">On hello **SAML Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="a0eef-178">a.</span><span class="sxs-lookup"><span data-stu-id="a0eef-178">a.</span></span> <span data-ttu-id="a0eef-179">V hello **e-mailovou doménu** textovému poli, zadejte registrované domény.</span><span class="sxs-lookup"><span data-stu-id="a0eef-179">In hello **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="a0eef-180">b.</span><span class="sxs-lookup"><span data-stu-id="a0eef-180">b.</span></span> <span data-ttu-id="a0eef-181">V **jeden přihlašovací adresa URL** textových polí, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a0eef-181">In  **Single Sign On URL** textboxes, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a0eef-182">c.</span><span class="sxs-lookup"><span data-stu-id="a0eef-182">c.</span></span> <span data-ttu-id="a0eef-183">Otevřete váš **soubor certifikátu** v poznámkovém bloku stáhli z portálu Azure, kopírovat obsah hello ho do schránky a pak ji vložit toohello **X509 certifikátu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a0eef-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="a0eef-184">d.</span><span class="sxs-lookup"><span data-stu-id="a0eef-184">d.</span></span> <span data-ttu-id="a0eef-185">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-185">Click **Save**.</span></span>

11. <span data-ttu-id="a0eef-186">V dialogovém okně Nastavení ověřování hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="a0eef-186">On hello Authentication Settings dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="a0eef-188">a.</span><span class="sxs-lookup"><span data-stu-id="a0eef-188">a.</span></span> <span data-ttu-id="a0eef-189">Klikněte na tlačítko **spuštění testu**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-189">Click **Run test**.</span></span>

    <span data-ttu-id="a0eef-190">b.</span><span class="sxs-lookup"><span data-stu-id="a0eef-190">b.</span></span> <span data-ttu-id="a0eef-191">Pokud hello hodnotu hello **stav** pole rovná **poslední stav testu: vytvoření**, obraťte vaše [tým podpory HackerOne](mailto:support@hackerone.com) toorequest o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a0eef-191">If hello value of hello **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) toorequest a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="a0eef-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="a0eef-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a0eef-193">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="a0eef-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a0eef-194">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a0eef-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a0eef-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0eef-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="a0eef-196">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="a0eef-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a0eef-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a0eef-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0eef-199">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a0eef-201">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a0eef-203">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="a0eef-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a0eef-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a0eef-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a0eef-207">a.</span><span class="sxs-lookup"><span data-stu-id="a0eef-207">a.</span></span> <span data-ttu-id="a0eef-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a0eef-209">b.</span><span class="sxs-lookup"><span data-stu-id="a0eef-209">b.</span></span> <span data-ttu-id="a0eef-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a0eef-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a0eef-211">c.</span><span class="sxs-lookup"><span data-stu-id="a0eef-211">c.</span></span> <span data-ttu-id="a0eef-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a0eef-213">d.</span><span class="sxs-lookup"><span data-stu-id="a0eef-213">d.</span></span> <span data-ttu-id="a0eef-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="a0eef-215">Vytvoření zkušebního uživatele HackerOne</span><span class="sxs-lookup"><span data-stu-id="a0eef-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="a0eef-216">Potom vytvořte uživatele v HackerOne nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0eef-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="a0eef-217">HackerOne podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="a0eef-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="a0eef-218">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="a0eef-218">There is no action item for you in this section.</span></span> <span data-ttu-id="a0eef-219">Při přístupu k HackerOne, je vytvořit nového uživatele, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="a0eef-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="a0eef-220">Pokud potřebujete toocreate uživatel ručně, je nutné tým podpory Certify toocontact hello.</span><span class="sxs-lookup"><span data-stu-id="a0eef-220">If you need toocreate a user manually, you need toocontact hello Certify support team.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a0eef-221">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a0eef-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a0eef-222">V této části povolíte tak, že udělíte přístup tooHackerOne toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a0eef-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHackerOne.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a0eef-224">**tooassign Britta Simon tooHackerOne, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a0eef-224">**tooassign Britta Simon tooHackerOne, perform hello following steps:**</span></span>

1. <span data-ttu-id="a0eef-225">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a0eef-227">V seznamu aplikace hello vyberte **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-227">In hello applications list, select **HackerOne**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="a0eef-229">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a0eef-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a0eef-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a0eef-231">Click **Add** button.</span></span> <span data-ttu-id="a0eef-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a0eef-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a0eef-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="a0eef-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a0eef-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a0eef-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a0eef-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a0eef-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a0eef-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a0eef-237">Testing single sign-on</span></span>

<span data-ttu-id="a0eef-238">Nakonec můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-238">Finally, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="a0eef-239">Když kliknete na dlaždici HackerOne hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour HackerOne aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0eef-239">When you click hello HackerOne tile in hello Access Panel, you should get automatically signed-on tooyour HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0eef-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a0eef-240">Additional resources</span></span>

* [<span data-ttu-id="a0eef-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a0eef-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a0eef-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a0eef-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

