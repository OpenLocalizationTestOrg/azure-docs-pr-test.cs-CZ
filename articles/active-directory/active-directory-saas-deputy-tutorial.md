---
title: "Kurz: Azure Active Directory integrace s zástupce | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a zástupce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 42f65b758682ce2513b6bb38ef40a19f955c88c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="f3017-103">Kurz: Azure Active Directory integrace s zástupce</span><span class="sxs-lookup"><span data-stu-id="f3017-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="f3017-104">V tomto kurzu zjistíte, jak toointegrate zástupce s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f3017-104">In this tutorial, you learn how toointegrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f3017-105">Integrace zástupce s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f3017-105">Integrating Deputy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f3017-106">Můžete řídit ve službě Azure AD, který má přístup tooDeputy</span><span class="sxs-lookup"><span data-stu-id="f3017-106">You can control in Azure AD who has access tooDeputy</span></span>
- <span data-ttu-id="f3017-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDeputy (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3017-107">You can enable your users tooautomatically get signed-on tooDeputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f3017-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3017-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f3017-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f3017-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3017-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3017-110">Prerequisites</span></span>

<span data-ttu-id="f3017-111">tooconfigure integrace Azure AD s zástupce, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f3017-111">tooconfigure Azure AD integration with Deputy, you need hello following items:</span></span>

- <span data-ttu-id="f3017-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3017-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f3017-113">Zástupce jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f3017-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f3017-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f3017-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f3017-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f3017-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f3017-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f3017-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f3017-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3017-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f3017-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f3017-118">Scenario description</span></span>
<span data-ttu-id="f3017-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f3017-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f3017-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f3017-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f3017-121">Přidání zástupce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f3017-121">Adding Deputy from hello gallery</span></span>
2. <span data-ttu-id="f3017-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3017-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-hello-gallery"></a><span data-ttu-id="f3017-123">Přidání zástupce z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f3017-123">Adding Deputy from hello gallery</span></span>
<span data-ttu-id="f3017-124">tooconfigure hello integrace zástupce do Azure AD, je nutné tooadd zástupce hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f3017-124">tooconfigure hello integration of Deputy into Azure AD, you need tooadd Deputy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f3017-125">**tooadd zástupce z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f3017-125">**tooadd Deputy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3017-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f3017-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f3017-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f3017-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f3017-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f3017-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f3017-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3017-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f3017-133">Hello vyhledávacího pole zadejte **zástupce**.</span><span class="sxs-lookup"><span data-stu-id="f3017-133">In hello search box, type **Deputy**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="f3017-135">Na panelu výsledků hello vyberte **zástupce**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3017-135">In hello results panel, select **Deputy**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f3017-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3017-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f3017-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s zástupce podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f3017-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f3017-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v zástupce je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f3017-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Deputy is tooa user in Azure AD.</span></span> <span data-ttu-id="f3017-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v zástupce musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f3017-140">In other words, a link relationship between an Azure AD user and hello related user in Deputy needs toobe established.</span></span>

<span data-ttu-id="f3017-141">V zástupce, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="f3017-141">In Deputy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f3017-142">tooconfigure a testu Azure AD jednotné přihlašování s zástupce, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f3017-142">tooconfigure and test Azure AD single sign-on with Deputy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f3017-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f3017-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f3017-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3017-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f3017-145">**[Vytvoření zkušebního uživatele zástupce](#creating-a-deputy-test-user)**  -toohave protějšek Britta Simon v zástupce, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="f3017-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - toohave a counterpart of Britta Simon in Deputy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f3017-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f3017-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f3017-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f3017-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f3017-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3017-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f3017-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci zástupce.</span><span class="sxs-lookup"><span data-stu-id="f3017-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="f3017-150">**tooconfigure Azure AD jednotné přihlašování s zástupce, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f3017-150">**tooconfigure Azure AD single sign-on with Deputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3017-151">V portálu Azure, na hello hello **zástupce** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f3017-151">In hello Azure portal, on hello **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f3017-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f3017-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="f3017-155">Na hello **zástupce domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="f3017-155">On hello **Deputy Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="f3017-157">a.</span><span class="sxs-lookup"><span data-stu-id="f3017-157">a.</span></span> <span data-ttu-id="f3017-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="f3017-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="f3017-159">b.</span><span class="sxs-lookup"><span data-stu-id="f3017-159">b.</span></span> <span data-ttu-id="f3017-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="f3017-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="f3017-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="f3017-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="f3017-162">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="f3017-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="f3017-164">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="f3017-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="f3017-165">Zástupce oblast přípona je volitelné, nebo se musí používat jednu z těchto: au | Ná | Evropa | jako | la | af | | ent au | ent na | ent Evropa | ent-jako | trola la | trola af | trola – element</span><span class="sxs-lookup"><span data-stu-id="f3017-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f3017-166">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f3017-166">These values are not real.</span></span> <span data-ttu-id="f3017-167">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="f3017-167">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="f3017-168">Obraťte se na [tým podpory zástupce](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3017-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) tooget these values.</span></span> 

5. <span data-ttu-id="f3017-169">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f3017-169">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="f3017-171">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3017-171">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="f3017-173">Na hello **zástupce konfigurace** klikněte na tlačítko **konfigurace zástupce** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f3017-173">On hello **Deputy Configuration** section, click **Configure Deputy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f3017-174">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f3017-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="f3017-176">Přejděte toohello následující adresu URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="f3017-176">Navigate toohello following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="f3017-177">Přejděte příliš**nastavení zabezpečení** a klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f3017-177">Go too**Security Settings** and click **Edit**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="f3017-179">V tomto **nastavení zabezpečení** proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="f3017-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="f3017-181">a.</span><span class="sxs-lookup"><span data-stu-id="f3017-181">a.</span></span> <span data-ttu-id="f3017-182">Povolit **přihlášení prostřednictvím sociální sítě**.</span><span class="sxs-lookup"><span data-stu-id="f3017-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="f3017-183">b.</span><span class="sxs-lookup"><span data-stu-id="f3017-183">b.</span></span> <span data-ttu-id="f3017-184">Otevřete váš certifikát kódovaný v Base64 stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **OpenSSL certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f3017-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="f3017-185">c.</span><span class="sxs-lookup"><span data-stu-id="f3017-185">c.</span></span> <span data-ttu-id="f3017-186">Do pole Adresa URL jednotné přihlašování SAML hello zadejte`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="f3017-186">In hello SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="f3017-187">d.</span><span class="sxs-lookup"><span data-stu-id="f3017-187">d.</span></span> <span data-ttu-id="f3017-188">V textovém poli hello URL jednotné přihlašování SAML, nahraďte `<your subdomain>` s vaší subdomény.</span><span class="sxs-lookup"><span data-stu-id="f3017-188">In hello SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="f3017-189">e.</span><span class="sxs-lookup"><span data-stu-id="f3017-189">e.</span></span> <span data-ttu-id="f3017-190">V textovém poli hello URL jednotné přihlašování SAML, nahraďte `<saml sso url>` s hello **SAML jeden přihlašování adresa URL služby** jste zkopírovali z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f3017-190">In hello SAML SSO URL textbox, replace `<saml sso url>` with hello **SAML Single Sign-On Service URL** you have copied from hello Azure portal.</span></span>
   
    <span data-ttu-id="f3017-191">f.</span><span class="sxs-lookup"><span data-stu-id="f3017-191">f.</span></span> <span data-ttu-id="f3017-192">Klikněte na tlačítko **uložit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="f3017-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="f3017-193">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="f3017-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f3017-194">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="f3017-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f3017-195">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f3017-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f3017-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3017-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="f3017-197">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="f3017-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f3017-199">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f3017-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3017-200">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f3017-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f3017-202">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f3017-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f3017-204">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="f3017-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f3017-206">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3017-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f3017-208">a.</span><span class="sxs-lookup"><span data-stu-id="f3017-208">a.</span></span> <span data-ttu-id="f3017-209">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f3017-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f3017-210">b.</span><span class="sxs-lookup"><span data-stu-id="f3017-210">b.</span></span> <span data-ttu-id="f3017-211">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f3017-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f3017-212">c.</span><span class="sxs-lookup"><span data-stu-id="f3017-212">c.</span></span> <span data-ttu-id="f3017-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f3017-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f3017-214">d.</span><span class="sxs-lookup"><span data-stu-id="f3017-214">d.</span></span> <span data-ttu-id="f3017-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f3017-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="f3017-216">Vytvoření zástupce zkušebního uživatele</span><span class="sxs-lookup"><span data-stu-id="f3017-216">Creating a Deputy test user</span></span>

<span data-ttu-id="f3017-217">Uživatelé toolog tooenable Azure AD v tooDeputy, se musí být zřízená do zástupce.</span><span class="sxs-lookup"><span data-stu-id="f3017-217">tooenable Azure AD users toolog in tooDeputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="f3017-218">V případě zástupce zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="f3017-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="f3017-219">tooprovision uživatelský účet, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="f3017-219">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="f3017-220">Přihlaste se na webu společnosti zástupce tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="f3017-220">Log in tooyour Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="f3017-221">V horním navigačním podokně hello klikněte **osoby**.</span><span class="sxs-lookup"><span data-stu-id="f3017-221">On hello top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="f3017-222">![Lidé](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="f3017-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="f3017-223">Klikněte na tlačítko hello **přidat osoby** tlačítko a klikněte na tlačítko **přidat jedna osoba**.</span><span class="sxs-lookup"><span data-stu-id="f3017-223">Click hello **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="f3017-224">![Přidat osoby](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="f3017-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="f3017-225">Proveďte následující kroky hello a klikněte na tlačítko **Uložit & Pozvat**.</span><span class="sxs-lookup"><span data-stu-id="f3017-225">Perform hello following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="f3017-226">![Nový uživatel](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="f3017-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="f3017-227">a.</span><span class="sxs-lookup"><span data-stu-id="f3017-227">a.</span></span> <span data-ttu-id="f3017-228">V hello **název** textovému poli, název typu hello uživatele jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f3017-228">In hello **Name** textbox, type name of hello user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="f3017-229">b.</span><span class="sxs-lookup"><span data-stu-id="f3017-229">b.</span></span> <span data-ttu-id="f3017-230">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu služby Azure AD má tooprovision.</span><span class="sxs-lookup"><span data-stu-id="f3017-230">In hello **Email** textbox, type hello email address of an Azure AD account you want tooprovision.</span></span>
   
   <span data-ttu-id="f3017-231">c.</span><span class="sxs-lookup"><span data-stu-id="f3017-231">c.</span></span> <span data-ttu-id="f3017-232">V hello **fungují na** textovému poli, název typu hello firmy.</span><span class="sxs-lookup"><span data-stu-id="f3017-232">In hello **Work at** textbox, type hello business name.</span></span>
   
   <span data-ttu-id="f3017-233">d.</span><span class="sxs-lookup"><span data-stu-id="f3017-233">d.</span></span> <span data-ttu-id="f3017-234">Klikněte na tlačítko **Uložit & Pozvat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3017-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="f3017-235">Držitel účtu AAD Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="f3017-235">hello AAD account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="f3017-236">Můžete použít všechny ostatní zástupce uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované zástupce tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f3017-236">You can use any other Deputy user account creation tools or APIs provided by Deputy tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f3017-237">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f3017-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f3017-238">V této části povolíte tak, že udělíte přístup tooDeputy toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f3017-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDeputy.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f3017-240">**tooassign Britta Simon tooDeputy, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f3017-240">**tooassign Britta Simon tooDeputy, perform hello following steps:**</span></span>

1. <span data-ttu-id="f3017-241">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f3017-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f3017-243">V seznamu aplikace hello vyberte **zástupce**.</span><span class="sxs-lookup"><span data-stu-id="f3017-243">In hello applications list, select **Deputy**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="f3017-245">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f3017-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f3017-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3017-247">Click **Add** button.</span></span> <span data-ttu-id="f3017-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3017-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f3017-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f3017-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f3017-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3017-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f3017-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3017-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f3017-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3017-253">Testing single sign-on</span></span>

<span data-ttu-id="f3017-254">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="f3017-254">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="f3017-255">Po kliknutí na tlačítko hello zástupce dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného zástupce aplikace.</span><span class="sxs-lookup"><span data-stu-id="f3017-255">When you click hello Deputy tile in hello Access Panel, you should get automatically signed-on tooyour Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f3017-256">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f3017-256">Additional resources</span></span>

* [<span data-ttu-id="f3017-257">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3017-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f3017-258">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f3017-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

