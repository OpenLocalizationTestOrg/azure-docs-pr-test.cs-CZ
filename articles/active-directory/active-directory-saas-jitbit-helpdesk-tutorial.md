---
title: 'Kurz: Azure Active Directory integrace s Jitbit Helpdesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Jitbit technickou podporu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f6c837bbb910c1e84f7ed9da676c5ab40f75338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a><span data-ttu-id="39c13-103">Kurz: Azure Active Directory integrace s Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="39c13-103">Tutorial: Azure Active Directory integration with Jitbit Helpdesk</span></span>

<span data-ttu-id="39c13-104">V tomto kurzu zjistíte, jak toointegrate Jitbit technickou podporu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="39c13-104">In this tutorial, you learn how toointegrate Jitbit Helpdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="39c13-105">Integrace Jitbit pracovník s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="39c13-105">Integrating Jitbit Helpdesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="39c13-106">Můžete řídit ve službě Azure AD, který má přístup tooJitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="39c13-106">You can control in Azure AD who has access tooJitbit Helpdesk</span></span>
- <span data-ttu-id="39c13-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooJitbit Helpdesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="39c13-107">You can enable your users tooautomatically get signed-on tooJitbit Helpdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="39c13-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="39c13-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="39c13-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="39c13-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39c13-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="39c13-110">Prerequisites</span></span>

<span data-ttu-id="39c13-111">tooconfigure integrace Azure AD s Jitbit Helpdesk, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="39c13-111">tooconfigure Azure AD integration with Jitbit Helpdesk, you need hello following items:</span></span>

- <span data-ttu-id="39c13-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="39c13-112">An Azure AD subscription</span></span>
- <span data-ttu-id="39c13-113">Jitbit Helpdesk jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="39c13-113">A Jitbit Helpdesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="39c13-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="39c13-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="39c13-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="39c13-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="39c13-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="39c13-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="39c13-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="39c13-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="39c13-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="39c13-118">Scenario description</span></span>
<span data-ttu-id="39c13-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="39c13-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="39c13-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="39c13-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="39c13-121">Přidání Jitbit Helpdesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="39c13-121">Adding Jitbit Helpdesk from hello gallery</span></span>
2. <span data-ttu-id="39c13-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="39c13-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jitbit-helpdesk-from-hello-gallery"></a><span data-ttu-id="39c13-123">Přidání Jitbit Helpdesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="39c13-123">Adding Jitbit Helpdesk from hello gallery</span></span>
<span data-ttu-id="39c13-124">tooconfigure hello integrace Jitbit Helpdesk do Azure AD, je nutné tooadd Jitbit Helpdesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="39c13-124">tooconfigure hello integration of Jitbit Helpdesk into Azure AD, you need tooadd Jitbit Helpdesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="39c13-125">**tooadd Jitbit Helpdesk z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="39c13-125">**tooadd Jitbit Helpdesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="39c13-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="39c13-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="39c13-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="39c13-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="39c13-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="39c13-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="39c13-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39c13-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="39c13-133">Hello vyhledávacího pole zadejte **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="39c13-133">In hello search box, type **Jitbit Helpdesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. <span data-ttu-id="39c13-135">Na panelu výsledků hello vyberte **Jitbit Helpdesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="39c13-135">In hello results panel, select **Jitbit Helpdesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="39c13-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="39c13-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="39c13-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s Jitbit Helpdesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="39c13-138">In this section, you configure and test Azure AD single sign-on with Jitbit Helpdesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="39c13-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Jitbit Helpdesk je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="39c13-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jitbit Helpdesk is tooa user in Azure AD.</span></span> <span data-ttu-id="39c13-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Jitbit Helpdesk musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="39c13-140">In other words, a link relationship between an Azure AD user and hello related user in Jitbit Helpdesk needs toobe established.</span></span>

<span data-ttu-id="39c13-141">V Jitbit Helpdesk přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="39c13-141">In Jitbit Helpdesk, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="39c13-142">tooconfigure a testu Azure AD jednotné přihlašování s Jitbit Helpdesk, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="39c13-142">tooconfigure and test Azure AD single sign-on with Jitbit Helpdesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="39c13-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="39c13-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="39c13-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="39c13-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="39c13-145">**[Vytvoření zkušebního uživatele Jitbit Helpdesk](#creating-a-jitbit-helpdesk-test-user)**  -toohave protějšek Britta Simon v Jitbit Helpdesk, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="39c13-145">**[Creating a Jitbit Helpdesk test user](#creating-a-jitbit-helpdesk-test-user)** - toohave a counterpart of Britta Simon in Jitbit Helpdesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="39c13-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39c13-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="39c13-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="39c13-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="39c13-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="39c13-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="39c13-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="39c13-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jitbit Helpdesk application.</span></span>

<span data-ttu-id="39c13-150">**tooconfigure Azure AD jednotné přihlašování s Jitbit Helpdesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="39c13-150">**tooconfigure Azure AD single sign-on with Jitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="39c13-151">V portálu Azure, na hello hello **Jitbit Helpdesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="39c13-151">In hello Azure portal, on hello **Jitbit Helpdesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="39c13-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39c13-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. <span data-ttu-id="39c13-155">Na hello **Jitbit Helpdesk domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="39c13-155">On hello **Jitbit Helpdesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    <span data-ttu-id="39c13-157">a.</span><span class="sxs-lookup"><span data-stu-id="39c13-157">a.</span></span> <span data-ttu-id="39c13-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="39c13-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > <span data-ttu-id="39c13-159">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="39c13-159">This value is not real.</span></span> <span data-ttu-id="39c13-160">Aktualizujte tuto hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39c13-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="39c13-161">Obraťte se na [tým podpory Jitbit Helpdesk klienta](https://www.jitbit.com/support/) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="39c13-161">Contact [Jitbit Helpdesk Client support team](https://www.jitbit.com/support/) tooget this value.</span></span> 
    
    <span data-ttu-id="39c13-162">b.</span><span class="sxs-lookup"><span data-stu-id="39c13-162">b.</span></span>  <span data-ttu-id="39c13-163">V hello **identifikátor** textovému poli, zadejte adresu URL jako následující:`https://www.jitbit.com/web-helpdesk/`</span><span class="sxs-lookup"><span data-stu-id="39c13-163">In hello **Identifier** textbox, type a URL as following: `https://www.jitbit.com/web-helpdesk/`</span></span>

    
 


4. <span data-ttu-id="39c13-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="39c13-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. <span data-ttu-id="39c13-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39c13-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="39c13-168">Na hello **Jitbit Helpdesk konfigurace** klikněte na tlačítko **konfigurace Jitbit Helpdesk** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="39c13-168">On hello **Jitbit Helpdesk Configuration** section, click **Configure Jitbit Helpdesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="39c13-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="39c13-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. <span data-ttu-id="39c13-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Jitbit Helpdesk.</span><span class="sxs-lookup"><span data-stu-id="39c13-171">In a different web browser window, log into your Jitbit Helpdesk company site as an administrator.</span></span>

8. <span data-ttu-id="39c13-172">V panelu nástrojů hello hello nahoře, klikněte na **správy**.</span><span class="sxs-lookup"><span data-stu-id="39c13-172">In hello toolbar on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="39c13-173">![Správa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "správy")</span><span class="sxs-lookup"><span data-stu-id="39c13-173">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

9. <span data-ttu-id="39c13-174">Klikněte na tlačítko **obecné nastavení**.</span><span class="sxs-lookup"><span data-stu-id="39c13-174">Click **General settings**.</span></span>
   
    <span data-ttu-id="39c13-175">![Uživatelé, společností a oprávnění](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "uživatelů, společností a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="39c13-175">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "Users, companies, and permissions")</span></span>

10. <span data-ttu-id="39c13-176">V hello **nastavení ověřování** konfigurace části, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="39c13-176">In hello **Authentication settings** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="39c13-177">![Nastavení ověřování](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "nastavení ověřování")</span><span class="sxs-lookup"><span data-stu-id="39c13-177">![Authentication settings](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "Authentication settings")</span></span>
    
    <span data-ttu-id="39c13-178">a.</span><span class="sxs-lookup"><span data-stu-id="39c13-178">a.</span></span> <span data-ttu-id="39c13-179">Vyberte **povolit SAML 2.0 jeden přihlásit**, toosign pomocí jednotné přihlašování (SSO), s **OneLogin**.</span><span class="sxs-lookup"><span data-stu-id="39c13-179">Select **Enable SAML 2.0 single sign on**, toosign in using Single Sign-On (SSO), with **OneLogin**.</span></span>

    <span data-ttu-id="39c13-180">b.</span><span class="sxs-lookup"><span data-stu-id="39c13-180">b.</span></span> <span data-ttu-id="39c13-181">V hello **adresu URL koncového bodu** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="39c13-181">In hello **EndPoint URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="39c13-182">c.</span><span class="sxs-lookup"><span data-stu-id="39c13-182">c.</span></span> <span data-ttu-id="39c13-183">Otevřete váš **kódování base-64** kódování certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textbox</span><span class="sxs-lookup"><span data-stu-id="39c13-183">Open your **base-64** encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>

    <span data-ttu-id="39c13-184">d.</span><span class="sxs-lookup"><span data-stu-id="39c13-184">d.</span></span> <span data-ttu-id="39c13-185">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="39c13-185">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="39c13-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="39c13-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="39c13-187">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="39c13-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="39c13-188">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="39c13-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="39c13-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="39c13-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="39c13-190">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="39c13-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="39c13-192">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="39c13-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="39c13-193">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="39c13-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="39c13-195">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="39c13-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="39c13-197">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="39c13-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="39c13-199">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="39c13-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="39c13-201">a.</span><span class="sxs-lookup"><span data-stu-id="39c13-201">a.</span></span> <span data-ttu-id="39c13-202">V hello **název** textovému poli, název typu jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="39c13-202">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="39c13-203">b.</span><span class="sxs-lookup"><span data-stu-id="39c13-203">b.</span></span> <span data-ttu-id="39c13-204">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="39c13-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="39c13-205">c.</span><span class="sxs-lookup"><span data-stu-id="39c13-205">c.</span></span> <span data-ttu-id="39c13-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="39c13-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="39c13-207">d.</span><span class="sxs-lookup"><span data-stu-id="39c13-207">d.</span></span> <span data-ttu-id="39c13-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="39c13-208">Click **Create**.</span></span>
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a><span data-ttu-id="39c13-209">Vytvoření zkušebního uživatele Jitbit Helpdesk</span><span class="sxs-lookup"><span data-stu-id="39c13-209">Creating a Jitbit Helpdesk test user</span></span>

<span data-ttu-id="39c13-210">V pořadí tooenable Azure AD Uživatelé toolog do Jitbit technickou podporu musí být zřízená do Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="39c13-210">In order tooenable Azure AD users toolog into Jitbit Helpdesk, they must be provisioned into Jitbit Helpdesk.</span></span>  <span data-ttu-id="39c13-211">V případě hello Jitbit Helpdesk zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="39c13-211">In hello case of Jitbit Helpdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="39c13-212">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="39c13-212">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="39c13-213">Přihlaste se tooyour **Jitbit Helpdesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="39c13-213">Log in tooyour **Jitbit Helpdesk** tenant.</span></span>

2. <span data-ttu-id="39c13-214">V nabídce hello hello nahoře, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="39c13-214">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="39c13-215">![Správa](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "správy")</span><span class="sxs-lookup"><span data-stu-id="39c13-215">![Administration](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "Administration")</span></span>

3. <span data-ttu-id="39c13-216">Klikněte na tlačítko **uživatelů, společností a oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="39c13-216">Click **Users, companies and permissions**.</span></span>
   
    <span data-ttu-id="39c13-217">![Uživatelé, společností a oprávnění](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "uživatelů, společností a oprávnění")</span><span class="sxs-lookup"><span data-stu-id="39c13-217">![Users, companies, and permissions](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "Users, companies, and permissions")</span></span>

4. <span data-ttu-id="39c13-218">Klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="39c13-218">Click **Add user**.</span></span>
   
    <span data-ttu-id="39c13-219">![Přidat uživatele](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="39c13-219">![Add user](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "Add user")</span></span>
   
5. <span data-ttu-id="39c13-220">V části vytvořit hello zadejte data hello hello Azure AD účet, že který má tooprovision následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="39c13-220">In hello Create section, type hello data of hello Azure AD account you want tooprovision as follows:</span></span>

    <span data-ttu-id="39c13-221">![Vytvoření](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="39c13-221">![Create](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "Create")</span></span>
   
   <span data-ttu-id="39c13-222">a.</span><span class="sxs-lookup"><span data-stu-id="39c13-222">a.</span></span> <span data-ttu-id="39c13-223">V hello **uživatelské jméno** textovému poli, typ **BrittaSimon**, hello uživatelské jméno jako hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="39c13-223">In hello **Username** textbox, type **BrittaSimon**, hello user name as in hello Azure portal.</span></span>

   <span data-ttu-id="39c13-224">b.</span><span class="sxs-lookup"><span data-stu-id="39c13-224">b.</span></span> <span data-ttu-id="39c13-225">V hello **e-mailu** jako typ e-mailu uživatele hello textovému poli,  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="39c13-225">In hello **Email** textbox, type email of hello user like **BrittaSimon@contoso.com**.</span></span>

   <span data-ttu-id="39c13-226">c.</span><span class="sxs-lookup"><span data-stu-id="39c13-226">c.</span></span> <span data-ttu-id="39c13-227">V hello **křestní jméno** textovému poli, typ křestní jméno uživatele hello jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="39c13-227">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>

   <span data-ttu-id="39c13-228">d.</span><span class="sxs-lookup"><span data-stu-id="39c13-228">d.</span></span> <span data-ttu-id="39c13-229">V hello **příjmení** textovému poli, typ příjmení uživatele hello jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="39c13-229">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span>
   
   <span data-ttu-id="39c13-230">e.</span><span class="sxs-lookup"><span data-stu-id="39c13-230">e.</span></span> <span data-ttu-id="39c13-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="39c13-231">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="39c13-232">Můžete použít jakékoli jiné Jitbit Helpdesk uživatele účtu vytvoření nástroje nebo rozhraní API poskytované Jitbit Helpdesk tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="39c13-232">You can use any other Jitbit Helpdesk user account creation tools or APIs provided by Jitbit Helpdesk tooprovision Azure AD user accounts.</span></span>
> 
        

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="39c13-233">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="39c13-233">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="39c13-234">V této části povolíte tak, že udělíte přístup tooJitbit Helpdesk Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="39c13-234">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJitbit Helpdesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="39c13-236">**tooassign tooJitbit Britta Simon technickou podporu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="39c13-236">**tooassign Britta Simon tooJitbit Helpdesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="39c13-237">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="39c13-237">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="39c13-239">V seznamu aplikace hello vyberte **Jitbit Helpdesk**.</span><span class="sxs-lookup"><span data-stu-id="39c13-239">In hello applications list, select **Jitbit Helpdesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. <span data-ttu-id="39c13-241">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="39c13-241">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="39c13-243">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39c13-243">Click **Add** button.</span></span> <span data-ttu-id="39c13-244">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39c13-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="39c13-246">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="39c13-246">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="39c13-247">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39c13-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="39c13-248">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="39c13-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="39c13-249">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="39c13-249">Testing single sign-on</span></span>

<span data-ttu-id="39c13-250">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="39c13-250">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="39c13-251">Po kliknutí na tlačítko hello Jitbit Helpdesk dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku aplikace Jitbit technickou podporu.</span><span class="sxs-lookup"><span data-stu-id="39c13-251">When you click hello Jitbit Helpdesk tile in hello Access Panel, you should get login page of Jitbit Helpdesk application.</span></span>
<span data-ttu-id="39c13-252">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="39c13-252">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39c13-253">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="39c13-253">Additional resources</span></span>

* [<span data-ttu-id="39c13-254">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39c13-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="39c13-255">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="39c13-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png

