---
title: "Kurz: Azure Active Directory integrace s Tichomoří rozvrhu | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a časových rozvrhů Tichomoří."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 5fd57ff78a3a64c135f2de9895f4643b39e33bb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a><span data-ttu-id="1557c-103">Kurz: Azure Active Directory integrace s Tichomoří časových rozvrhů</span><span class="sxs-lookup"><span data-stu-id="1557c-103">Tutorial: Azure Active Directory integration with Pacific Timesheet</span></span>

<span data-ttu-id="1557c-104">V tomto kurzu zjistíte, jak toointegrate časových rozvrhů Tichomoří službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1557c-104">In this tutorial, you learn how toointegrate Pacific Timesheet with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1557c-105">Integrace Tichomoří rozvrhu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1557c-105">Integrating Pacific Timesheet with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1557c-106">Můžete řídit ve službě Azure AD, který má přístup tooPacific časových rozvrhů</span><span class="sxs-lookup"><span data-stu-id="1557c-106">You can control in Azure AD who has access tooPacific Timesheet</span></span>
- <span data-ttu-id="1557c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPacific časových rozvrhů (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="1557c-107">You can enable your users tooautomatically get signed-on tooPacific Timesheet (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1557c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1557c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="1557c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1557c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1557c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1557c-110">Prerequisites</span></span>

<span data-ttu-id="1557c-111">tooconfigure integrace Azure AD s Tichomoří rozvrhu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="1557c-111">tooconfigure Azure AD integration with Pacific Timesheet, you need hello following items:</span></span>

- <span data-ttu-id="1557c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1557c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1557c-113">Časových rozvrhů Tichomoří jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1557c-113">A Pacific Timesheet single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1557c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1557c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1557c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1557c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1557c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1557c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1557c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1557c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1557c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1557c-118">Scenario description</span></span>
<span data-ttu-id="1557c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1557c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1557c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1557c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1557c-121">Přidání Tichomoří rozvrhu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1557c-121">Adding Pacific Timesheet from hello gallery</span></span>
2. <span data-ttu-id="1557c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1557c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pacific-timesheet-from-hello-gallery"></a><span data-ttu-id="1557c-123">Přidání Tichomoří rozvrhu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="1557c-123">Adding Pacific Timesheet from hello gallery</span></span>
<span data-ttu-id="1557c-124">tooconfigure hello integrace Tichomoří rozvrhu do Azure AD, je nutné tooadd Tichomoří rozvrhu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1557c-124">tooconfigure hello integration of Pacific Timesheet into Azure AD, you need tooadd Pacific Timesheet from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1557c-125">**tooadd časových rozvrhů Tichomoří z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1557c-125">**tooadd Pacific Timesheet from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1557c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1557c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1557c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1557c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1557c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1557c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="1557c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1557c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="1557c-133">Hello vyhledávacího pole zadejte **Tichomoří rozvrhu**.</span><span class="sxs-lookup"><span data-stu-id="1557c-133">In hello search box, type **Pacific Timesheet**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

5. <span data-ttu-id="1557c-135">Na panelu výsledků hello vyberte **Tichomoří rozvrhu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1557c-135">In hello results panel, select **Pacific Timesheet**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1557c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1557c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1557c-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí časových rozvrhů Tichomoří podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1557c-138">In this section, you configure and test Azure AD single sign-on with Pacific Timesheet based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1557c-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Tichomoří rozvrhu je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1557c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pacific Timesheet is tooa user in Azure AD.</span></span> <span data-ttu-id="1557c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Tichomoří rozvrhu musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="1557c-140">In other words, a link relationship between an Azure AD user and hello related user in Pacific Timesheet needs toobe established.</span></span>

<span data-ttu-id="1557c-141">V časových rozvrhů Tichomoří, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="1557c-141">In Pacific Timesheet, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1557c-142">tooconfigure a testu Azure AD jednotné přihlašování s Tichomoří rozvrhu, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="1557c-142">tooconfigure and test Azure AD single sign-on with Pacific Timesheet, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1557c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="1557c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1557c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1557c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1557c-145">**[Vytvoření zkušebního uživatele Tichomoří rozvrhu](#creating-a-pacific-timesheet-test-user)**  -toohave protějšek Britta Simon v Tichomoří rozvrhu, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="1557c-145">**[Creating a Pacific Timesheet test user](#creating-a-pacific-timesheet-test-user)** - toohave a counterpart of Britta Simon in Pacific Timesheet that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1557c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1557c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1557c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="1557c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1557c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1557c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1557c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Tichomoří rozvrhu.</span><span class="sxs-lookup"><span data-stu-id="1557c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Pacific Timesheet application.</span></span>

<span data-ttu-id="1557c-150">**tooconfigure Azure AD jednotné přihlašování s Tichomoří rozvrhu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1557c-150">**tooconfigure Azure AD single sign-on with Pacific Timesheet, perform hello following steps:**</span></span>

1. <span data-ttu-id="1557c-151">V portálu Azure, na hello hello **Tichomoří rozvrhu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1557c-151">In hello Azure portal, on hello **Pacific Timesheet** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="1557c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1557c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

3. <span data-ttu-id="1557c-155">Na hello **Tichomoří časových rozvrhů domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="1557c-155">On hello **Pacific Timesheet Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    <span data-ttu-id="1557c-157">a.</span><span class="sxs-lookup"><span data-stu-id="1557c-157">a.</span></span> <span data-ttu-id="1557c-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="1557c-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    <span data-ttu-id="1557c-159">b.</span><span class="sxs-lookup"><span data-stu-id="1557c-159">b.</span></span> <span data-ttu-id="1557c-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="1557c-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1557c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="1557c-161">These values are not real.</span></span> <span data-ttu-id="1557c-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1557c-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="1557c-163">Obraťte se na [tým podpory Tichomoří rozvrhu](http://www.pacifictimesheet.com/support) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1557c-163">Contact [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) tooget these values.</span></span>
 
4. <span data-ttu-id="1557c-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="1557c-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

5. <span data-ttu-id="1557c-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1557c-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1557c-168">Na hello **konfigurace časových rozvrhů Tichomoří** klikněte na tlačítko **konfigurace časových rozvrhů Tichomoří** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1557c-168">On hello **Pacific Timesheet Configuration** section, click **Configure Pacific Timesheet** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1557c-169">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1557c-169">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

7. <span data-ttu-id="1557c-171">tooconfigure jednotného přihlašování na **Tichomoří rozvrhu** straně, je nutné stáhnout hello toosend **certifikátu (Base64)**, **SAML jeden přihlašování adresa URL služby**a  **SAML Entity ID** příliš[tým podpory Tichomoří rozvrhu](http://www.pacifictimesheet.com/support).</span><span class="sxs-lookup"><span data-stu-id="1557c-171">tooconfigure single sign-on on **Pacific Timesheet** side, you need toosend hello downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** too[Pacific Timesheet support team](http://www.pacifictimesheet.com/support).</span></span> <span data-ttu-id="1557c-172">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="1557c-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="1557c-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="1557c-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1557c-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="1557c-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1557c-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1557c-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1557c-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1557c-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="1557c-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="1557c-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="1557c-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1557c-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1557c-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1557c-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1557c-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1557c-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1557c-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="1557c-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1557c-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1557c-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1557c-188">a.</span><span class="sxs-lookup"><span data-stu-id="1557c-188">a.</span></span> <span data-ttu-id="1557c-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1557c-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1557c-190">b.</span><span class="sxs-lookup"><span data-stu-id="1557c-190">b.</span></span> <span data-ttu-id="1557c-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1557c-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1557c-192">c.</span><span class="sxs-lookup"><span data-stu-id="1557c-192">c.</span></span> <span data-ttu-id="1557c-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="1557c-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="1557c-194">d.</span><span class="sxs-lookup"><span data-stu-id="1557c-194">d.</span></span> <span data-ttu-id="1557c-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1557c-195">Click **Create**.</span></span>
 
### <a name="creating-a-pacific-timesheet-test-user"></a><span data-ttu-id="1557c-196">Vytvoření zkušebního uživatele Tichomoří časových rozvrhů</span><span class="sxs-lookup"><span data-stu-id="1557c-196">Creating a Pacific Timesheet test user</span></span>

<span data-ttu-id="1557c-197">V této části vytvoříte uživatele volat Britta Simon v Tichomoří rozvrhu.</span><span class="sxs-lookup"><span data-stu-id="1557c-197">In this section, you create a user called Britta Simon in Pacific Timesheet.</span></span> <span data-ttu-id="1557c-198">Práce s [tým podpory Tichomoří rozvrhu](http://www.pacifictimesheet.com/support) toocreate uživatele v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="1557c-198">Work with [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) toocreate a user in hello application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1557c-199">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="1557c-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="1557c-200">V této části povolíte tak, že udělíte přístup tooPacific časových rozvrhů Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1557c-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPacific Timesheet.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="1557c-202">**tooassign tooPacific Britta Simon časových rozvrhů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="1557c-202">**tooassign Britta Simon tooPacific Timesheet, perform hello following steps:**</span></span>

1. <span data-ttu-id="1557c-203">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1557c-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1557c-205">V seznamu aplikace hello vyberte **Tichomoří rozvrhu**.</span><span class="sxs-lookup"><span data-stu-id="1557c-205">In hello applications list, select **Pacific Timesheet**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

3. <span data-ttu-id="1557c-207">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1557c-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="1557c-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1557c-209">Click **Add** button.</span></span> <span data-ttu-id="1557c-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1557c-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="1557c-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="1557c-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1557c-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1557c-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1557c-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1557c-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1557c-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1557c-215">Testing single sign-on</span></span>

<span data-ttu-id="1557c-216">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1557c-216">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1557c-217">Když kliknete na dlaždici Tichomoří rozvrhu hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Tichomoří rozvrhu aplikace.</span><span class="sxs-lookup"><span data-stu-id="1557c-217">When you click hello Pacific Timesheet tile in hello Access Panel, you should get automatically signed-on tooyour Pacific Timesheet application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1557c-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1557c-218">Additional resources</span></span>

* [<span data-ttu-id="1557c-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1557c-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1557c-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1557c-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png

