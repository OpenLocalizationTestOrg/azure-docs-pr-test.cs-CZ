---
title: 'Kurz: Azure Active Directory integrace s SumoLogic | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SumoLogic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 2ef1bd329f5db8899f0b57744e4c0f6eed1c532f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="7d4a0-103">Kurz: Azure Active Directory integrace s SumoLogic</span><span class="sxs-lookup"><span data-stu-id="7d4a0-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="7d4a0-104">V tomto kurzu zjistíte, jak toointegrate SumoLogic s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7d4a0-104">In this tutorial, you learn how toointegrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7d4a0-105">Integrace SumoLogic s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-105">Integrating SumoLogic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7d4a0-106">Můžete řídit ve službě Azure AD, který má přístup tooSumoLogic</span><span class="sxs-lookup"><span data-stu-id="7d4a0-106">You can control in Azure AD who has access tooSumoLogic</span></span>
- <span data-ttu-id="7d4a0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSumoLogic (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d4a0-107">You can enable your users tooautomatically get signed-on tooSumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7d4a0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7d4a0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7d4a0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7d4a0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d4a0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7d4a0-110">Prerequisites</span></span>

<span data-ttu-id="7d4a0-111">Integrace služby Azure AD s SumoLogic tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-111">tooconfigure Azure AD integration with SumoLogic, you need hello following items:</span></span>

- <span data-ttu-id="7d4a0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d4a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7d4a0-113">SumoLogic jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7d4a0-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7d4a0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7d4a0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7d4a0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7d4a0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7d4a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7d4a0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7d4a0-118">Scenario description</span></span>
<span data-ttu-id="7d4a0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7d4a0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7d4a0-121">Přidání SumoLogic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7d4a0-121">Adding SumoLogic from hello gallery</span></span>
2. <span data-ttu-id="7d4a0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d4a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-hello-gallery"></a><span data-ttu-id="7d4a0-123">Přidání SumoLogic z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7d4a0-123">Adding SumoLogic from hello gallery</span></span>
<span data-ttu-id="7d4a0-124">tooconfigure hello integrace SumoLogic do Azure AD, je nutné tooadd SumoLogic hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-124">tooconfigure hello integration of SumoLogic into Azure AD, you need tooadd SumoLogic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7d4a0-125">**tooadd SumoLogic z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-125">**tooadd SumoLogic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d4a0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7d4a0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7d4a0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7d4a0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7d4a0-133">Hello vyhledávacího pole zadejte **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-133">In hello search box, type **SumoLogic**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="7d4a0-135">Na panelu výsledků hello vyberte **SumoLogic**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-135">In hello results panel, select **SumoLogic**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7d4a0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d4a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7d4a0-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SumoLogic podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7d4a0-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7d4a0-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v SumoLogic je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SumoLogic is tooa user in Azure AD.</span></span> <span data-ttu-id="7d4a0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SumoLogic musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-140">In other words, a link relationship between an Azure AD user and hello related user in SumoLogic needs toobe established.</span></span>

<span data-ttu-id="7d4a0-141">V SumoLogic, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-141">In SumoLogic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7d4a0-142">tooconfigure a testu Azure AD jednotné přihlašování s SumoLogic, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-142">tooconfigure and test Azure AD single sign-on with SumoLogic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7d4a0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7d4a0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7d4a0-145">**[Vytvoření zkušebního uživatele SumoLogic](#creating-a-sumologic-test-user)**  -toohave protějšek Britta Simon v SumoLogic, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - toohave a counterpart of Britta Simon in SumoLogic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7d4a0-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7d4a0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7d4a0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d4a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7d4a0-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="7d4a0-150">**tooconfigure Azure AD jednotné přihlašování s SumoLogic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-150">**tooconfigure Azure AD single sign-on with SumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d4a0-151">V portálu Azure, na hello hello **SumoLogic** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-151">In hello Azure portal, on hello **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7d4a0-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="7d4a0-155">Na hello **SumoLogic domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-155">On hello **SumoLogic Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="7d4a0-157">a.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-157">a.</span></span> <span data-ttu-id="7d4a0-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="7d4a0-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="7d4a0-159">b.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-159">b.</span></span> <span data-ttu-id="7d4a0-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="7d4a0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-161">These values are not real.</span></span> <span data-ttu-id="7d4a0-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7d4a0-163">Obraťte se na [tým podpory SumoLogic klienta](https://www.sumologic.com/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="7d4a0-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="7d4a0-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7d4a0-168">Na hello **SumoLogic konfigurace** klikněte na tlačítko **konfigurace SumoLogic** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-168">On hello **SumoLogic Configuration** section, click **Configure SumoLogic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7d4a0-169">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="7d4a0-171">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti SumoLogic tooyour.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-171">In a different web browser window, log in tooyour SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="7d4a0-172">Přejděte příliš**spravovat \> zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-172">Go too**Manage \> Security**.</span></span>
   
    <span data-ttu-id="7d4a0-173">![Spravovat](./media/active-directory-saas-sumologic-tutorial/ic778556.png "spravovat")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="7d4a0-174">Klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="7d4a0-175">![Nastavení zabezpečení globální](./media/active-directory-saas-sumologic-tutorial/ic778557.png "nastavení globálního zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="7d4a0-176">Z hello **vyberte konfiguraci, nebo vytvořte novou** vyberte **Azure AD**a potom klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-176">From hello **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="7d4a0-177">![Konfigurace SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "konfigurace SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="7d4a0-178">Na hello **konfigurace SAML 2.0** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-178">On hello **Configure SAML 2.0** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="7d4a0-179">![Konfigurace SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "konfigurace SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="7d4a0-180">a.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-180">a.</span></span> <span data-ttu-id="7d4a0-181">V hello **název konfigurace** textovému poli, typ **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-181">In hello **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="7d4a0-182">b.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-182">b.</span></span> <span data-ttu-id="7d4a0-183">Vyberte **režim ladění**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="7d4a0-184">c.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-184">c.</span></span> <span data-ttu-id="7d4a0-185">V hello **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-185">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="7d4a0-186">d.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-186">d.</span></span> <span data-ttu-id="7d4a0-187">V hello **ověřovacího požadavku URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-187">In hello **Authn Request URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="7d4a0-188">e.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-188">e.</span></span> <span data-ttu-id="7d4a0-189">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="7d4a0-190">f.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-190">f.</span></span> <span data-ttu-id="7d4a0-191">Jako **atribut e-mailu**, vyberte **pomocí SAML subjektu**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="7d4a0-192">g.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-192">g.</span></span> <span data-ttu-id="7d4a0-193">Vyberte **SP iniciované konfigurace přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="7d4a0-194">h.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-194">h.</span></span> <span data-ttu-id="7d4a0-195">V hello **přihlašovací cestu** textovému poli, typ **Azure** a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-195">In hello **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7d4a0-196">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7d4a0-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7d4a0-197">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7d4a0-198">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7d4a0-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7d4a0-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d4a0-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="7d4a0-200">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7d4a0-202">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d4a0-203">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7d4a0-205">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7d4a0-207">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7d4a0-209">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7d4a0-211">a.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-211">a.</span></span> <span data-ttu-id="7d4a0-212">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7d4a0-213">b.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-213">b.</span></span> <span data-ttu-id="7d4a0-214">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7d4a0-215">c.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-215">c.</span></span> <span data-ttu-id="7d4a0-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7d4a0-217">d.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-217">d.</span></span> <span data-ttu-id="7d4a0-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="7d4a0-219">Vytvoření zkušebního uživatele SumoLogic</span><span class="sxs-lookup"><span data-stu-id="7d4a0-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="7d4a0-220">V pořadí tooenable Azure AD Uživatelé toolog v tooSumoLogic musí být zřízená tooSumoLogic.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-220">In order tooenable Azure AD users toolog in tooSumoLogic, they must be provisioned tooSumoLogic.</span></span>  

* <span data-ttu-id="7d4a0-221">V případě hello SumoLogic zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-221">In hello case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="7d4a0-222">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-222">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d4a0-223">Přihlaste se tooyour **SumoLogic** klienta.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-223">Log in tooyour **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="7d4a0-224">Přejděte příliš**spravovat \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-224">Go too**Manage \> Users**.</span></span>
   
    <span data-ttu-id="7d4a0-225">![Uživatelé](./media/active-directory-saas-sumologic-tutorial/ic778561.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="7d4a0-226">Klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-226">Click **Add**.</span></span>
   
    <span data-ttu-id="7d4a0-227">![Uživatelé](./media/active-directory-saas-sumologic-tutorial/ic778562.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="7d4a0-228">Na hello **nového uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7d4a0-228">On hello **New User** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="7d4a0-229">![Nový uživatel](./media/active-directory-saas-sumologic-tutorial/ic778563.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="7d4a0-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="7d4a0-230">a.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-230">a.</span></span> <span data-ttu-id="7d4a0-231">Typ hello související informace účtu hello Azure AD má tooprovision do hello **křestní jméno**, **příjmení**, a **e-mailu** textových polí.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-231">Type hello related information of hello Azure AD account you want tooprovision into hello **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="7d4a0-232">b.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-232">b.</span></span> <span data-ttu-id="7d4a0-233">Vyberte roli.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-233">Select a role.</span></span>
  
    <span data-ttu-id="7d4a0-234">c.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-234">c.</span></span> <span data-ttu-id="7d4a0-235">Jako **stav**, vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="7d4a0-236">d.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-236">d.</span></span> <span data-ttu-id="7d4a0-237">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="7d4a0-238">Můžete použít všechny ostatní SumoLogic uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SumoLogic tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7d4a0-239">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7d4a0-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7d4a0-240">V této části povolíte tak, že udělíte přístup tooSumoLogic toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSumoLogic.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7d4a0-242">**tooassign Britta Simon tooSumoLogic, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7d4a0-242">**tooassign Britta Simon tooSumoLogic, perform hello following steps:**</span></span>

1. <span data-ttu-id="7d4a0-243">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7d4a0-245">V seznamu aplikace hello vyberte **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-245">In hello applications list, select **SumoLogic**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="7d4a0-247">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7d4a0-249">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-249">Click **Add** button.</span></span> <span data-ttu-id="7d4a0-250">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7d4a0-252">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7d4a0-253">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7d4a0-254">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7d4a0-255">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7d4a0-255">Testing single sign-on</span></span>

<span data-ttu-id="7d4a0-256">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7d4a0-257">Když kliknete na dlaždici SumoLogic hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SumoLogic aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d4a0-257">When you click hello SumoLogic tile in hello Access Panel, you should get automatically signed-on tooyour SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d4a0-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7d4a0-258">Additional resources</span></span>

* [<span data-ttu-id="7d4a0-259">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d4a0-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7d4a0-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d4a0-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

