---
title: 'Kurz: Azure Active Directory integrace s Adobe Creative cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Adobe Creative cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="9d0ff-103">Kurz: Azure Active Directory integrace s Adobe Creative cloudu</span><span class="sxs-lookup"><span data-stu-id="9d0ff-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="9d0ff-104">V tomto kurzu zjistíte, jak toointegrate Adobe Creative cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9d0ff-104">In this tutorial, you learn how toointegrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9d0ff-105">Integrace Adobe Creative cloudu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-105">Integrating Adobe Creative Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9d0ff-106">Můžete řídit ve službě Azure AD, který má přístup tooAdobe tvůrčí cloudu</span><span class="sxs-lookup"><span data-stu-id="9d0ff-106">You can control in Azure AD who has access tooAdobe Creative Cloud</span></span>
- <span data-ttu-id="9d0ff-107">Vaši uživatelé tooautomatically get přihlášeného tooAdobe tvůrčí cloudu (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="9d0ff-107">You can enable your users tooautomatically get signed-on tooAdobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9d0ff-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="9d0ff-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="9d0ff-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9d0ff-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d0ff-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9d0ff-110">Prerequisites</span></span>

<span data-ttu-id="9d0ff-111">tooconfigure integrace Azure AD s Adobe Creative cloudu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-111">tooconfigure Azure AD integration with Adobe Creative Cloud, you need hello following items:</span></span>

- <span data-ttu-id="9d0ff-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9d0ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9d0ff-113">Tvůrčí cloudu Adobe jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9d0ff-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9d0ff-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9d0ff-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9d0ff-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9d0ff-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9d0ff-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9d0ff-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9d0ff-118">Scenario description</span></span>
<span data-ttu-id="9d0ff-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9d0ff-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9d0ff-121">Přidání Adobe Creative cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9d0ff-121">Adding Adobe Creative Cloud from hello gallery</span></span>
2. <span data-ttu-id="9d0ff-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9d0ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a><span data-ttu-id="9d0ff-123">Přidání Adobe Creative cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9d0ff-123">Adding Adobe Creative Cloud from hello gallery</span></span>
<span data-ttu-id="9d0ff-124">tooconfigure hello Integrace Adobe Creative cloudu do Azure AD, je nutné tooadd Adobe Creative cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-124">tooconfigure hello integration of Adobe Creative Cloud into Azure AD, you need tooadd Adobe Creative Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9d0ff-125">**tooadd Adobe Creative cloudu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9d0ff-125">**tooadd Adobe Creative Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9d0ff-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9d0ff-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9d0ff-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9d0ff-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9d0ff-133">Hello vyhledávacího pole zadejte **Adobe Creative cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-133">In hello search box, type **Adobe Creative Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="9d0ff-135">Na panelu výsledků hello vyberte **Adobe Creative cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-135">In hello results panel, select **Adobe Creative Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9d0ff-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9d0ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9d0ff-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Adobe Creative cloudu podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9d0ff-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9d0ff-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu tvůrčí Adobe je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Creative Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="9d0ff-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu tvůrčí Adobe musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Creative Cloud needs toobe established.</span></span>

<span data-ttu-id="9d0ff-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="9d0ff-142">tooconfigure a testu Azure AD jednotné přihlašování s Adobe Creative cloudu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-142">tooconfigure and test Azure AD single sign-on with Adobe Creative Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9d0ff-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9d0ff-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9d0ff-145">**[Vytváření testovacího uživatele Adobe Creative cloudu](#creating-an-adobe-creative-cloud-test-user)**  -toohave protějšek Britta Simon Adobe Creative cloudu, který je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - toohave a counterpart of Britta Simon in Adobe Creative Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9d0ff-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9d0ff-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9d0ff-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9d0ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9d0ff-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="9d0ff-150">**tooconfigure Azure AD jednotné přihlašování s Adobe Creative cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9d0ff-150">**tooconfigure Azure AD single sign-on with Adobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="9d0ff-151">V hello Azure Management portal na hello **Adobe Creative cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-151">In hello Azure Management portal, on hello **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9d0ff-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="9d0ff-155">Na hello **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-155">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="9d0ff-157">a.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-157">a.</span></span> <span data-ttu-id="9d0ff-158">V hello **identifikátor** textovému poli, hodnota typu hello jako:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="9d0ff-158">In hello **Identifier** textbox, type hello value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="9d0ff-159">b.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-159">b.</span></span> <span data-ttu-id="9d0ff-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="9d0ff-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9d0ff-161">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="9d0ff-162">Máte tooupdate tyto hodnoty pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="9d0ff-163">Zde, doporučujeme vám toouse hello jedinečnou hodnotu řetězce v hello identifikátor.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="9d0ff-164">Pokud potřebujete toocreate uživatelé ručně, je nutné tým podpory Adobe Creative cloudu toocontact hello.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-164">If you need toocreate an user manually, you need toocontact hello Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="9d0ff-165">Na hello **Adobe Creative cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-165">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="9d0ff-167">a.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-167">a.</span></span> <span data-ttu-id="9d0ff-168">Klikněte na hello **zobrazit upřesňující nastavení adresy URL** možnost</span><span class="sxs-lookup"><span data-stu-id="9d0ff-168">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="9d0ff-169">b.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-169">b.</span></span> <span data-ttu-id="9d0ff-170">V hello **přihlašovací adresa URL** textovému poli, hodnota typu hello jako:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="9d0ff-170">In hello **Sign-on URL** textbox, type hello value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="9d0ff-171">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-171">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="9d0ff-173">Na hello **Adobe Creative konfigurace cloudu** klikněte na tlačítko **konfigurace cloudu tvůrčí Adobe** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-173">On hello **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9d0ff-174">Zkopírujte hello **SAML Entity Id** a **adresa URL služby Jednotné přihlašování SAML** z Stručná referenční příručka oddílu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-174">Please copy hello **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="9d0ff-176">V okně prohlížeče jiný web, klienta Adobe Creative cloudových tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-176">In a different web browser window, sign-on tooyour Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="9d0ff-177">Přejděte příliš**Identity** hello levém navigačním podokně a klikněte na doménu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-177">Go too**Identity** on hello left navigation pane and click your domain.</span></span> <span data-ttu-id="9d0ff-178">Potom proveďte následující kroky hello **jeden znak na konfigurace požadované** části.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-178">Then perform hello following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="9d0ff-179">![Nastavení](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="9d0ff-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="9d0ff-180">Klikněte na tlačítko **Procházet** tooupload hello stáhnout certifikát z Azure AD příliš**IDP certifikát**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-180">Click **Browse** tooupload hello downloaded certificate from Azure AD too**IDP Certificate**.</span></span>

10. <span data-ttu-id="9d0ff-181">V hello **IDP vystavitele** textovému poli, vložte hodnotu hello z **SAML Entity Id** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-181">In hello **IDP issuer** textbox, put hello value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="9d0ff-182">V hello **IDP přihlašovací adresa URL** textovému poli, vložte hodnotu hello z **adresa URL služby Jednotné přihlašování SAML** který jste zkopírovali ze **konfigurovat přihlášení** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-182">In hello **IDP Login URL** textbox, put hello value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="9d0ff-183">Vyberte **HTTP - přesměrování** jako **IDP vazby**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="9d0ff-184">Vyberte **e-mailová adresa** jako **nastavení přihlášení uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="9d0ff-185">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-185">Click **Save** button.</span></span>

15. <span data-ttu-id="9d0ff-186">řídicí panel Hello teď nabídne hello XML **"Stáhnout Metadata"** souboru.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-186">hello dashboard will now present hello XML **"Download Metadata"** file.</span></span> <span data-ttu-id="9d0ff-187">Obsahuje EntityDescriptor adresy URL a adresy URL AssertionConsumerService na Adobe.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="9d0ff-188">Otevřete soubor hello a nakonfigurujte je hello aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-188">Please open hello file and configure them in hello Azure AD application.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="9d0ff-191">a.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-191">a.</span></span> <span data-ttu-id="9d0ff-192">Použití hello EntityDescriptor hodnota Adobe zadaný pro **identifikátor** na hello **nakonfigurovat nastavení aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-192">Use hello EntityDescriptor value Adobe provided you for **Identifier** on hello **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="9d0ff-193">b.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-193">b.</span></span> <span data-ttu-id="9d0ff-194">Použití hello AssertionConsumerService hodnota Adobe zadaný pro **adresa URL odpovědi** na hello **nakonfigurovat nastavení aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-194">Use hello AssertionConsumerService value Adobe provided you for **Reply URL** on hello **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9d0ff-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9d0ff-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="9d0ff-196">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-196">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9d0ff-198">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9d0ff-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9d0ff-199">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-199">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9d0ff-201">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-201">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9d0ff-203">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9d0ff-205">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9d0ff-207">a.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-207">a.</span></span> <span data-ttu-id="9d0ff-208">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9d0ff-209">b.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-209">b.</span></span> <span data-ttu-id="9d0ff-210">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9d0ff-211">c.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-211">c.</span></span> <span data-ttu-id="9d0ff-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9d0ff-213">d.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-213">d.</span></span> <span data-ttu-id="9d0ff-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="9d0ff-215">Vytváření testovacího uživatele Adobe Creative cloudu</span><span class="sxs-lookup"><span data-stu-id="9d0ff-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="9d0ff-216">V pořadí tooenable Azure AD Uživatelé toolog do Adobe Creative cloudu musí být zřízená do Adobe Creative cloudu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-216">In order tooenable Azure AD users toolog into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="9d0ff-217">V případě hello Adobe Creative cloudu zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-217">In hello case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="9d0ff-218">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9d0ff-218">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="9d0ff-219">Přihlaste se tooyour Adobe Creative cloudu na webu společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-219">Log in tooyour Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="9d0ff-220">Klikněte na tlačítko **osoby**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-220">Click **People**.</span></span>

    <span data-ttu-id="9d0ff-221">![Lidé](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "osoby")</span><span class="sxs-lookup"><span data-stu-id="9d0ff-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="9d0ff-222">Klikněte na tlačítko **pozvat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-222">Click **Invite User**.</span></span>

    <span data-ttu-id="9d0ff-223">![Uživatele pozvat](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="9d0ff-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="9d0ff-224">Na hello **pozvat osoby** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9d0ff-224">On hello **Invite People** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="9d0ff-225">![Pozvěte lidi, kteří](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "pozvat uživatele")</span><span class="sxs-lookup"><span data-stu-id="9d0ff-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="9d0ff-226">a.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-226">a.</span></span> <span data-ttu-id="9d0ff-227">V hello **e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-227">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="9d0ff-228">b.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-228">b.</span></span> <span data-ttu-id="9d0ff-229">Klikněte na tlačítko **pozvat**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d0ff-230">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9d0ff-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9d0ff-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9d0ff-232">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooAdobe tvůrčí cloudu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAdobe Creative Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9d0ff-234">**tooassign tooAdobe Britta Simon tvůrčí cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9d0ff-234">**tooassign Britta Simon tooAdobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="9d0ff-235">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-235">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9d0ff-237">V seznamu aplikace hello vyberte **Adobe Creative cloudu**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-237">In hello applications list, select **Adobe Creative Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="9d0ff-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9d0ff-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-241">Click **Add** button.</span></span> <span data-ttu-id="9d0ff-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9d0ff-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9d0ff-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9d0ff-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9d0ff-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9d0ff-247">Testing single sign-on</span></span>

<span data-ttu-id="9d0ff-248">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9d0ff-249">Když kliknete na dlaždici Adobe Creative cloudu hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Adobe Creative cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="9d0ff-249">When you click hello Adobe Creative Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9d0ff-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9d0ff-250">Additional resources</span></span>

* [<span data-ttu-id="9d0ff-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9d0ff-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9d0ff-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9d0ff-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png