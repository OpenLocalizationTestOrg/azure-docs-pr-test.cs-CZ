---
title: 'Kurz: Azure Active Directory integrace s Nexonia | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Nexonia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.openlocfilehash: 3778804084a7989414260bae5654cf76e9ccca6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a><span data-ttu-id="4bf32-103">Kurz: Azure Active Directory integrace s Nexonia</span><span class="sxs-lookup"><span data-stu-id="4bf32-103">Tutorial: Azure Active Directory integration with Nexonia</span></span>

<span data-ttu-id="4bf32-104">V tomto kurzu zjistíte, jak toointegrate Nexonia s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4bf32-104">In this tutorial, you learn how toointegrate Nexonia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4bf32-105">Integrace Nexonia s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="4bf32-105">Integrating Nexonia with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4bf32-106">Můžete řídit ve službě Azure AD, který má přístup tooNexonia</span><span class="sxs-lookup"><span data-stu-id="4bf32-106">You can control in Azure AD who has access tooNexonia</span></span>
- <span data-ttu-id="4bf32-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNexonia (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bf32-107">You can enable your users tooautomatically get signed-on tooNexonia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4bf32-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="4bf32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4bf32-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="4bf32-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="4bf32-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4bf32-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4bf32-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4bf32-111">Prerequisites</span></span>

<span data-ttu-id="4bf32-112">Integrace služby Azure AD s Nexonia tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="4bf32-112">tooconfigure Azure AD integration with Nexonia, you need hello following items:</span></span>

- <span data-ttu-id="4bf32-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bf32-113">An Azure AD subscription</span></span>
- <span data-ttu-id="4bf32-114">Nexonia jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="4bf32-114">A Nexonia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4bf32-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="4bf32-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4bf32-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="4bf32-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4bf32-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="4bf32-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4bf32-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4bf32-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4bf32-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="4bf32-119">Scenario description</span></span>
<span data-ttu-id="4bf32-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="4bf32-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4bf32-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="4bf32-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4bf32-122">Přidání Nexonia z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4bf32-122">Adding Nexonia from hello gallery</span></span>
2. <span data-ttu-id="4bf32-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4bf32-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nexonia-from-hello-gallery"></a><span data-ttu-id="4bf32-124">Přidání Nexonia z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="4bf32-124">Adding Nexonia from hello gallery</span></span>
<span data-ttu-id="4bf32-125">tooconfigure hello integrace Nexonia do Azure AD, je nutné tooadd Nexonia hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="4bf32-125">tooconfigure hello integration of Nexonia into Azure AD, you need tooadd Nexonia from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4bf32-126">**tooadd Nexonia z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4bf32-126">**tooadd Nexonia from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4bf32-127">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4bf32-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4bf32-129">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4bf32-130">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-130">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="4bf32-132">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bf32-132">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="4bf32-134">Hello vyhledávacího pole zadejte **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-134">In hello search box, type **Nexonia**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. <span data-ttu-id="4bf32-136">Na panelu výsledků hello vyberte **Nexonia**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="4bf32-136">In hello results panel, select **Nexonia**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4bf32-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="4bf32-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4bf32-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nexonia podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="4bf32-139">In this section, you configure and test Azure AD single sign-on with Nexonia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4bf32-140">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Nexonia je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bf32-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nexonia is tooa user in Azure AD.</span></span> <span data-ttu-id="4bf32-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Nexonia musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="4bf32-141">In other words, a link relationship between an Azure AD user and hello related user in Nexonia needs toobe established.</span></span>

<span data-ttu-id="4bf32-142">V Nexonia, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="4bf32-142">In Nexonia, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4bf32-143">tooconfigure a testu Azure AD jednotné přihlašování s Nexonia, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="4bf32-143">tooconfigure and test Azure AD single sign-on with Nexonia, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4bf32-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="4bf32-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4bf32-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4bf32-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4bf32-146">**[Vytvoření zkušebního uživatele Nexonia](#creating-a-nexonia-test-user)**  -toohave protějšek Britta Simon v Nexonia, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4bf32-146">**[Creating a Nexonia test user](#creating-a-nexonia-test-user)** - toohave a counterpart of Britta Simon in Nexonia that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4bf32-147">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4bf32-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4bf32-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="4bf32-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4bf32-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4bf32-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4bf32-150">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Nexonia.</span><span class="sxs-lookup"><span data-stu-id="4bf32-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nexonia application.</span></span>

>[!Note]
><span data-ttu-id="4bf32-151">Pokud máte problémy v hello integrace a odkazovat to [odkaz](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) pro Průvodce odstraňováním potíží s.</span><span class="sxs-lookup"><span data-stu-id="4bf32-151">If you have issues in hello integration then refer this [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) for troubleshooting guide.</span></span> <span data-ttu-id="4bf32-152">Pokud stále nebyly nalezeny hello řešení, pak vyvolat žádost o podporu hello z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4bf32-152">If you still have not found hello solution, then raise hello support request from Azure portal.</span></span>

<span data-ttu-id="4bf32-153">**tooconfigure Azure AD jednotné přihlašování s Nexonia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4bf32-153">**tooconfigure Azure AD single sign-on with Nexonia, perform hello following steps:**</span></span>

1. <span data-ttu-id="4bf32-154">V portálu Azure, na hello hello **Nexonia** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-154">In hello Azure portal, on hello **Nexonia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="4bf32-156">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4bf32-156">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. <span data-ttu-id="4bf32-158">Na hello **Nexonia domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="4bf32-158">On hello **Nexonia Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    <span data-ttu-id="4bf32-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span><span class="sxs-lookup"><span data-stu-id="4bf32-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4bf32-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="4bf32-161">This value is not real.</span></span> <span data-ttu-id="4bf32-162">Aktualizujte tuto hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="4bf32-162">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="4bf32-163">Obraťte se na [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bf32-163">Contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) tooget this value.</span></span> 


4. <span data-ttu-id="4bf32-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="4bf32-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. <span data-ttu-id="4bf32-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bf32-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4bf32-168">Na hello **Nexonia konfigurace** klikněte na tlačítko **konfigurace Nexonia** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="4bf32-168">On hello **Nexonia Configuration** section, click **Configure Nexonia** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4bf32-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="4bf32-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. <span data-ttu-id="4bf32-171">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, obraťte se na [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) a poskytnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bf32-171">tooget SSO configured for your application, contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) and provide them with hello following:</span></span>

    <span data-ttu-id="4bf32-172">• hello Stáhnout **certifikátu**</span><span class="sxs-lookup"><span data-stu-id="4bf32-172">• hello downloaded **certificate**</span></span>

    <span data-ttu-id="4bf32-173">• hello **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="4bf32-173">• hello **SAML Entity ID**</span></span>

    <span data-ttu-id="4bf32-174">• hello **SAML jeden přihlašování adresa URL služby**</span><span class="sxs-lookup"><span data-stu-id="4bf32-174">• hello **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="4bf32-175">• hello **Sign-Out adresy URL**</span><span class="sxs-lookup"><span data-stu-id="4bf32-175">• hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="4bf32-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="4bf32-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4bf32-177">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="4bf32-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4bf32-178">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4bf32-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4bf32-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bf32-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="4bf32-180">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="4bf32-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="4bf32-182">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4bf32-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4bf32-183">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="4bf32-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4bf32-185">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4bf32-187">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="4bf32-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4bf32-189">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4bf32-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4bf32-191">a.</span><span class="sxs-lookup"><span data-stu-id="4bf32-191">a.</span></span> <span data-ttu-id="4bf32-192">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4bf32-193">b.</span><span class="sxs-lookup"><span data-stu-id="4bf32-193">b.</span></span> <span data-ttu-id="4bf32-194">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4bf32-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4bf32-195">c.</span><span class="sxs-lookup"><span data-stu-id="4bf32-195">c.</span></span> <span data-ttu-id="4bf32-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4bf32-197">d.</span><span class="sxs-lookup"><span data-stu-id="4bf32-197">d.</span></span> <span data-ttu-id="4bf32-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-198">Click **Create**.</span></span>
 
### <a name="creating-a-nexonia-test-user"></a><span data-ttu-id="4bf32-199">Vytvoření zkušebního uživatele Nexonia</span><span class="sxs-lookup"><span data-stu-id="4bf32-199">Creating a Nexonia test user</span></span>

<span data-ttu-id="4bf32-200">V této části vytvoříte volal Britta Simon v Nexonia uživatele.</span><span class="sxs-lookup"><span data-stu-id="4bf32-200">In this section, you create a user called Britta Simon in Nexonia.</span></span> <span data-ttu-id="4bf32-201">Práce s [tým podpory Nexonia](https://nexonia.zendesk.com/hc/requests/new) tooadd hello uživatelé v platformě Nexonia hello.</span><span class="sxs-lookup"><span data-stu-id="4bf32-201">Work with [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) tooadd hello users in hello Nexonia platform.</span></span> <span data-ttu-id="4bf32-202">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4bf32-202">Users must be created and activated before you use single sign-on.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4bf32-203">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="4bf32-203">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4bf32-204">V této části povolíte tak, že udělíte přístup tooNexonia toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="4bf32-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNexonia.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="4bf32-206">**tooassign Britta Simon tooNexonia, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="4bf32-206">**tooassign Britta Simon tooNexonia, perform hello following steps:**</span></span>

1. <span data-ttu-id="4bf32-207">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="4bf32-209">V seznamu aplikace hello vyberte **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-209">In hello applications list, select **Nexonia**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. <span data-ttu-id="4bf32-211">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="4bf32-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="4bf32-213">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bf32-213">Click **Add** button.</span></span> <span data-ttu-id="4bf32-214">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4bf32-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="4bf32-216">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="4bf32-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4bf32-217">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4bf32-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4bf32-218">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4bf32-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4bf32-219">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="4bf32-219">Testing single sign-on</span></span>

<span data-ttu-id="4bf32-220">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="4bf32-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4bf32-221">Když kliknete na dlaždici Nexonia hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Nexonia aplikace.</span><span class="sxs-lookup"><span data-stu-id="4bf32-221">When you click hello Nexonia tile in hello Access Panel, you should get automatically signed-on tooyour Nexonia application.</span></span>
<span data-ttu-id="4bf32-222">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="4bf32-222">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4bf32-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="4bf32-223">Additional resources</span></span>

* [<span data-ttu-id="4bf32-224">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4bf32-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4bf32-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4bf32-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png

