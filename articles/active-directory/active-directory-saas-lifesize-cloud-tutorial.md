---
title: 'Kurz: Azure Active Directory integrace s Lifesize cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Lifesize cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="5459b-103">Kurz: Azure Active Directory integrace s Lifesize cloudu</span><span class="sxs-lookup"><span data-stu-id="5459b-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="5459b-104">V tomto kurzu zjistíte, jak toointegrate Lifesize cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5459b-104">In this tutorial, you learn how toointegrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5459b-105">Integrace Lifesize cloudu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5459b-105">Integrating Lifesize Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5459b-106">Můžete řídit ve službě Azure AD, který má přístup tooLifesize cloudu</span><span class="sxs-lookup"><span data-stu-id="5459b-106">You can control in Azure AD who has access tooLifesize Cloud</span></span>
- <span data-ttu-id="5459b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooLifesize cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5459b-107">You can enable your users tooautomatically get signed-on tooLifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5459b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5459b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5459b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5459b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5459b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5459b-110">Prerequisites</span></span>

<span data-ttu-id="5459b-111">tooconfigure integrace Azure AD s Lifesize cloudu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5459b-111">tooconfigure Azure AD integration with Lifesize Cloud, you need hello following items:</span></span>

- <span data-ttu-id="5459b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5459b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5459b-113">Cloudu Lifesize jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5459b-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5459b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5459b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5459b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5459b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5459b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5459b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5459b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5459b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5459b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5459b-118">Scenario description</span></span>
<span data-ttu-id="5459b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5459b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5459b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5459b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5459b-121">Přidání Lifesize cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5459b-121">Adding Lifesize Cloud from hello gallery</span></span>
2. <span data-ttu-id="5459b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5459b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-hello-gallery"></a><span data-ttu-id="5459b-123">Přidání Lifesize cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5459b-123">Adding Lifesize Cloud from hello gallery</span></span>
<span data-ttu-id="5459b-124">tooconfigure hello integrace Lifesize cloudu do Azure AD, je nutné tooadd Lifesize cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5459b-124">tooconfigure hello integration of Lifesize Cloud into Azure AD, you need tooadd Lifesize Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5459b-125">**tooadd Lifesize cloudu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5459b-125">**tooadd Lifesize Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5459b-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5459b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5459b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5459b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5459b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5459b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5459b-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5459b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5459b-133">Hello vyhledávacího pole zadejte **Lifesize cloudu**.</span><span class="sxs-lookup"><span data-stu-id="5459b-133">In hello search box, type **Lifesize Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="5459b-135">Na panelu výsledků hello vyberte **Lifesize cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5459b-135">In hello results panel, select **Lifesize Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5459b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5459b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5459b-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s cloudem Lifesize podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5459b-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5459b-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu Lifesize je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5459b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Lifesize Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="5459b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu Lifesize musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5459b-140">In other words, a link relationship between an Azure AD user and hello related user in Lifesize Cloud needs toobe established.</span></span>

<span data-ttu-id="5459b-141">V cloudu Lifesize přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5459b-141">In Lifesize Cloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5459b-142">tooconfigure a testu Azure AD jednotné přihlašování s Lifesize cloudu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="5459b-142">tooconfigure and test Azure AD single sign-on with Lifesize Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5459b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5459b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5459b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5459b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5459b-145">**[Vytvoření zkušebního uživatele Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  -toohave protějšek Britta Simon Lifesize cloudu, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5459b-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - toohave a counterpart of Britta Simon in Lifesize Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5459b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5459b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5459b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5459b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5459b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5459b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5459b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Lifesize cloudu.</span><span class="sxs-lookup"><span data-stu-id="5459b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="5459b-150">**tooconfigure Azure AD jednotné přihlašování s Lifesize cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5459b-150">**tooconfigure Azure AD single sign-on with Lifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="5459b-151">V portálu Azure, na hello hello **Lifesize cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5459b-151">In hello Azure portal, on hello **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5459b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5459b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="5459b-155">Na hello **Lifesize cloudové domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5459b-155">On hello **Lifesize Cloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="5459b-157">a.</span><span class="sxs-lookup"><span data-stu-id="5459b-157">a.</span></span> <span data-ttu-id="5459b-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="5459b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="5459b-159">b.</span><span class="sxs-lookup"><span data-stu-id="5459b-159">b.</span></span> <span data-ttu-id="5459b-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="5459b-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="5459b-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, proveďte následující krok hello:</span><span class="sxs-lookup"><span data-stu-id="5459b-161">Check **Show advanced URL settings**, perform hello following step:</span></span>  
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="5459b-163">V hello **předávání stavu** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="5459b-163">In hello **Relay state** textbox, type a URL using hello following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="5459b-164">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5459b-164">Please note that these are not hello real values.</span></span> <span data-ttu-id="5459b-165">Máte tooupdate tyto hodnoty s hello skutečná adresa URL přihlašování, stav směrování a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5459b-165">you have tooupdate these values with hello actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="5459b-166">Obraťte se na [tým podpory klient Cloud Lifesize](https://www.lifesize.com/support) tooget přihlašovací adresa URL a identifikátor hodnoty a můžete získat hodnotu předávání stavu z konfigurace jednotného přihlašování, která je vysvětlená později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) tooget Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in hello tutorial.</span></span>

4. <span data-ttu-id="5459b-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5459b-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="5459b-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5459b-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5459b-171">Na hello **konfigurace cloudu Lifesize** klikněte na tlačítko **konfigurace cloudu Lifesize** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5459b-171">On hello **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5459b-172">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5459b-172">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="5459b-174">tooget nakonfigurovat jednotné přihlašování pro vaši aplikaci, přihlásit se k hello Lifesize cloudových aplikací s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="5459b-174">tooget SSO configured for your application, login into hello Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="5459b-175">V hello pravém horním rohu klikněte na název a potom klikněte na hello **rozšířená nastavení**.</span><span class="sxs-lookup"><span data-stu-id="5459b-175">In hello top right corner click on your name and then click on hello **Advance Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="5459b-177">V hello rozšířená nastavení klikněte na na hello **Konfigurace jednotného přihlašování k** odkaz.</span><span class="sxs-lookup"><span data-stu-id="5459b-177">In hello Advance Settings now click on hello **SSO Configuration** link.</span></span> <span data-ttu-id="5459b-178">Otevře stránku hello Konfigurace jednotného přihlašování pro vaše instance.</span><span class="sxs-lookup"><span data-stu-id="5459b-178">It will open hello SSO Configuration page for your instance.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="5459b-180">Teď nakonfigurujte hello následující hodnoty v konfiguraci jednotného přihlašování k hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5459b-180">Now configure hello following values in hello SSO configuration UI.</span></span>    
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="5459b-182">a.</span><span class="sxs-lookup"><span data-stu-id="5459b-182">a.</span></span> <span data-ttu-id="5459b-183">V **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5459b-183">In **Identity Provider Issuer** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5459b-184">b.</span><span class="sxs-lookup"><span data-stu-id="5459b-184">b.</span></span>  <span data-ttu-id="5459b-185">V **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5459b-185">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="5459b-186">c.</span><span class="sxs-lookup"><span data-stu-id="5459b-186">c.</span></span> <span data-ttu-id="5459b-187">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="5459b-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="5459b-188">d.</span><span class="sxs-lookup"><span data-stu-id="5459b-188">d.</span></span> <span data-ttu-id="5459b-189">Hello SAML atribut mapování pro hello křestní jméno textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="5459b-189">In hello SAML Attribute mappings for hello First Name text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="5459b-190">e.</span><span class="sxs-lookup"><span data-stu-id="5459b-190">e.</span></span> <span data-ttu-id="5459b-191">V mapování hello atribut SAML pro hello **příjmení** textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="5459b-191">In hello SAML Attribute mapping for hello **Last Name** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="5459b-192">f.</span><span class="sxs-lookup"><span data-stu-id="5459b-192">f.</span></span> <span data-ttu-id="5459b-193">V mapování hello atribut SAML pro hello **e-mailu** textového pole zadejte hodnotu hello jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="5459b-193">In hello SAML Attribute mapping for hello **Email** text box enter hello value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="5459b-194">Konfigurace hello toocheck můžete kliknutím na hello **Test** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5459b-194">toocheck hello configuration you can click on hello **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="5459b-195">Pro úspěšné testování potřebovat Průvodce konfigurací hello toocomplete ve službě Azure AD a také poskytnout přístup toousers nebo skupin, které můžete provést hello test.</span><span class="sxs-lookup"><span data-stu-id="5459b-195">For successful testing you need toocomplete hello configuration wizard in Azure AD and also provide access toousers or groups who can perform hello test.</span></span>

12. <span data-ttu-id="5459b-196">Povolit hello jednotné přihlašování kontrolou na hello **povolit jednotné přihlašování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5459b-196">Enable hello SSO by checking on hello **Enable SSO** button.</span></span>

13. <span data-ttu-id="5459b-197">Nyní klikněte na hello **aktualizace** tlačítko tak, aby se ukládají všechna nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-197">Now click on hello **Update** button so that all hello settings are saved.</span></span> <span data-ttu-id="5459b-198">Tím se vygeneruje hello RelayState hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5459b-198">This will generate hello RelayState value.</span></span> <span data-ttu-id="5459b-199">Kopírování hello RelayState hodnotu, která se generují hello textového pole, vložte jej v hello **předávání stavu** textového pole pod **Lifesize cloudové domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="5459b-199">Copy hello RelayState value, which is generated in hello text box, paste it in hello **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="5459b-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5459b-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5459b-201">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5459b-202">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5459b-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5459b-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5459b-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="5459b-204">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5459b-206">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5459b-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5459b-207">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5459b-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5459b-209">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5459b-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5459b-211">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5459b-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5459b-213">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5459b-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5459b-215">a.</span><span class="sxs-lookup"><span data-stu-id="5459b-215">a.</span></span> <span data-ttu-id="5459b-216">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5459b-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5459b-217">b.</span><span class="sxs-lookup"><span data-stu-id="5459b-217">b.</span></span> <span data-ttu-id="5459b-218">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5459b-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5459b-219">c.</span><span class="sxs-lookup"><span data-stu-id="5459b-219">c.</span></span> <span data-ttu-id="5459b-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5459b-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5459b-221">d.</span><span class="sxs-lookup"><span data-stu-id="5459b-221">d.</span></span> <span data-ttu-id="5459b-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5459b-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="5459b-223">Vytvoření zkušebního uživatele Lifesize cloudu</span><span class="sxs-lookup"><span data-stu-id="5459b-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="5459b-224">V této části vytvoříte uživatele názvem Britta Simon v Lifesize cloudu.</span><span class="sxs-lookup"><span data-stu-id="5459b-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="5459b-225">Lifesize cloudu podporovat zřizování automatické uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5459b-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="5459b-226">Po úspěšném ověření v Azure AD se automaticky zřídí hello uživatele v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-226">After successful authentication at Azure AD, hello user will be automatically provisioned in hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5459b-227">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5459b-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5459b-228">V této části povolíte tak, že udělíte přístup tooLifesize cloudu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5459b-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLifesize Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5459b-230">**tooassign tooLifesize Britta Simon cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5459b-230">**tooassign Britta Simon tooLifesize Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="5459b-231">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5459b-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5459b-233">V seznamu aplikace hello vyberte **Lifesize cloudu**.</span><span class="sxs-lookup"><span data-stu-id="5459b-233">In hello applications list, select **Lifesize Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="5459b-235">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5459b-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5459b-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5459b-237">Click **Add** button.</span></span> <span data-ttu-id="5459b-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5459b-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5459b-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5459b-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5459b-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5459b-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5459b-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5459b-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5459b-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5459b-243">Testing single sign-on</span></span>

<span data-ttu-id="5459b-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5459b-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5459b-245">Po kliknutí na tlačítko hello Lifesize cloudu dlaždici v hello přístupového panelu, měli byste obdržet přihlašovací stránku Lifesize cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="5459b-245">When you click hello Lifesize Cloud tile in hello Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="5459b-246">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5459b-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5459b-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5459b-247">Additional resources</span></span>

* [<span data-ttu-id="5459b-248">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5459b-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5459b-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5459b-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

