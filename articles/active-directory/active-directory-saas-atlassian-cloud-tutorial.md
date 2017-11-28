---
title: 'Kurz: Azure Active Directory integrace s Atlassian cloudu | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Atlassian cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a><span data-ttu-id="88752-103">Kurz: Azure Active Directory integrace s Atlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="88752-103">Tutorial: Azure Active Directory integration with Atlassian Cloud</span></span>

<span data-ttu-id="88752-104">V tomto kurzu zjistíte, jak toointegrate Atlassian cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="88752-104">In this tutorial, you learn how toointegrate Atlassian Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="88752-105">Integrace Atlassian cloudu s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="88752-105">Integrating Atlassian Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="88752-106">Můžete řídit ve službě Azure AD, který má přístup tooAtlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="88752-106">You can control in Azure AD who has access tooAtlassian Cloud</span></span>
- <span data-ttu-id="88752-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooAtlassian cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="88752-107">You can enable your users tooautomatically get signed-on tooAtlassian Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="88752-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="88752-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="88752-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="88752-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88752-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="88752-110">Prerequisites</span></span>

<span data-ttu-id="88752-111">tooconfigure integrace Azure AD s Atlassian cloudu, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="88752-111">tooconfigure Azure AD integration with Atlassian Cloud, you need hello following items:</span></span>

- <span data-ttu-id="88752-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="88752-112">An Azure AD subscription</span></span>
- <span data-ttu-id="88752-113">Cloudu Atlassian jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="88752-113">An Atlassian Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="88752-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="88752-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="88752-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="88752-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="88752-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="88752-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="88752-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="88752-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="88752-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="88752-118">Scenario description</span></span>
<span data-ttu-id="88752-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="88752-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="88752-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="88752-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="88752-121">Přidání Atlassian cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="88752-121">Adding Atlassian Cloud from hello gallery</span></span>
2. <span data-ttu-id="88752-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="88752-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atlassian-cloud-from-hello-gallery"></a><span data-ttu-id="88752-123">Přidání Atlassian cloudu z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="88752-123">Adding Atlassian Cloud from hello gallery</span></span>
<span data-ttu-id="88752-124">tooconfigure hello integrace Atlassian cloudu do Azure AD, je nutné tooadd Atlassian cloudu hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="88752-124">tooconfigure hello integration of Atlassian Cloud into Azure AD, you need tooadd Atlassian Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="88752-125">**tooadd Atlassian cloudu z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="88752-125">**tooadd Atlassian Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="88752-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="88752-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="88752-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="88752-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="88752-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="88752-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="88752-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88752-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="88752-133">Hello vyhledávacího pole zadejte **Atlassian cloudu**.</span><span class="sxs-lookup"><span data-stu-id="88752-133">In hello search box, type **Atlassian Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. <span data-ttu-id="88752-135">Na panelu výsledků hello vyberte **Atlassian cloudu**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="88752-135">In hello results panel, select **Atlassian Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="88752-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="88752-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="88752-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s Atlassian cloudu podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="88752-138">In this section, you configure and test Azure AD single sign-on with Atlassian Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="88752-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu Atlassian je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="88752-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Atlassian Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="88752-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu Atlassian musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="88752-140">In other words, a link relationship between an Azure AD user and hello related user in Atlassian Cloud needs toobe established.</span></span>

<span data-ttu-id="88752-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="88752-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Atlassian Cloud.</span></span>

<span data-ttu-id="88752-142">tooconfigure a testu Azure AD jednotné přihlašování s Atlassian cloudu, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="88752-142">tooconfigure and test Azure AD single sign-on with Atlassian Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="88752-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="88752-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="88752-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="88752-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="88752-145">**[Vytváření testovacího uživatele cloudu Atlassian](#creating-an-atlassian-cloud-test-user)**  -toohave protějšek Britta Simon Atlassian cloudu, který je propojený toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="88752-145">**[Creating an Atlassian Cloud test user](#creating-an-atlassian-cloud-test-user)** - toohave a counterpart of Britta Simon in Atlassian Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="88752-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88752-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="88752-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="88752-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="88752-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="88752-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="88752-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="88752-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Atlassian Cloud application.</span></span>

<span data-ttu-id="88752-150">**tooconfigure Azure AD jednotné přihlašování s Atlassian cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="88752-150">**tooconfigure Azure AD single sign-on with Atlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="88752-151">V portálu Azure, na hello hello **Atlassian cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="88752-151">In hello Azure portal, on hello **Atlassian Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="88752-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88752-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. <span data-ttu-id="88752-155">Na hello **Atlassian cloudové domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="88752-155">On hello **Atlassian Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    <span data-ttu-id="88752-157">a.</span><span class="sxs-lookup"><span data-stu-id="88752-157">a.</span></span> <span data-ttu-id="88752-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.atlassian.net/admin/saml/edit`</span><span class="sxs-lookup"><span data-stu-id="88752-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net/admin/saml/edit`</span></span>

    <span data-ttu-id="88752-159">b.</span><span class="sxs-lookup"><span data-stu-id="88752-159">b.</span></span> <span data-ttu-id="88752-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL jako:`https://id.atlassian.com/login/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="88752-160">In hello **Reply URL** textbox, type a URL as: `https://id.atlassian.com/login/saml/acs`</span></span>

4. <span data-ttu-id="88752-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL** a proveďte následující krok, pokud chcete aplikace hello tooconfigure hello **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="88752-161">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    <span data-ttu-id="88752-163">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<instancename>.atlassian.net`</span><span class="sxs-lookup"><span data-stu-id="88752-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="88752-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="88752-164">These values are not real.</span></span> <span data-ttu-id="88752-165">Aktualizovat tyto hodnoty s hello skutečné identifikátor a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="88752-165">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="88752-166">Přesné hodnoty hello můžete získat z obrazovky konfigurace SAML Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="88752-166">You can get hello exact values from Atlassian Cloud SAML Configuration screen.</span></span>
 
5. <span data-ttu-id="88752-167">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="88752-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. <span data-ttu-id="88752-169">Na hello **konfigurace cloudu Atlassian** klikněte na tlačítko **konfigurace cloudu Atlassian** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="88752-169">On hello **Atlassian Cloud Configuration** section, click **Configure Atlassian Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="88752-170">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="88752-170">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. <span data-ttu-id="88752-172">tooget jednotné přihlašování nakonfigurovat pro vaši aplikaci, přihlášení toohello Atlassian portálu pomocí hello práva správce.</span><span class="sxs-lookup"><span data-stu-id="88752-172">tooget SSO configured for your application, login toohello Atlassian Portal using hello administrator rights.</span></span>

8. <span data-ttu-id="88752-173">V části ověřování hello hello levé navigační na **domény**.</span><span class="sxs-lookup"><span data-stu-id="88752-173">In hello Authentication section of hello left navigation click **Domains**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    <span data-ttu-id="88752-175">a.</span><span class="sxs-lookup"><span data-stu-id="88752-175">a.</span></span> <span data-ttu-id="88752-176">Hello textovému poli, zadejte název domény a pak klikněte na **přidáním domény**.</span><span class="sxs-lookup"><span data-stu-id="88752-176">In hello textbox, type your domain name, and then click **Add domain**.</span></span>
        
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    <span data-ttu-id="88752-178">b.</span><span class="sxs-lookup"><span data-stu-id="88752-178">b.</span></span> <span data-ttu-id="88752-179">tooverify hello domény, klikněte na tlačítko **ověřte**.</span><span class="sxs-lookup"><span data-stu-id="88752-179">tooverify hello domain, click **Verify**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    <span data-ttu-id="88752-181">c.</span><span class="sxs-lookup"><span data-stu-id="88752-181">c.</span></span> <span data-ttu-id="88752-182">Stáhnout soubor html ověření domény hello, nahrajte ho toohello kořenové složky vaší doméně webu a pak klikněte na tlačítko **ověřit doménu**.</span><span class="sxs-lookup"><span data-stu-id="88752-182">Download hello domain verification html file, upload it toohello root folder of your domain's website, and then click **Verify domain**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    <span data-ttu-id="88752-184">d.</span><span class="sxs-lookup"><span data-stu-id="88752-184">d.</span></span> <span data-ttu-id="88752-185">Po ověření domény hello hello hodnotu hello **stav** pole je **ověřeno**.</span><span class="sxs-lookup"><span data-stu-id="88752-185">Once hello domain is verified, hello value of hello **Status** field is **Verified**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. <span data-ttu-id="88752-187">V levém navigačním panelu hello, klikněte na tlačítko **SAML**.</span><span class="sxs-lookup"><span data-stu-id="88752-187">In hello left navigation bar, click **SAML**.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. <span data-ttu-id="88752-189">Vytvoření konfigurace SAML a přidejte hello konfigurace zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="88752-189">Create a SAML Configuration and add hello Identity provider configuration.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    <span data-ttu-id="88752-191">a.</span><span class="sxs-lookup"><span data-stu-id="88752-191">a.</span></span> <span data-ttu-id="88752-192">V hello **zprostředkovatele Identity Entity ID** textového pole, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="88752-192">In hello **Identity provider Entity ID** text box, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="88752-193">b.</span><span class="sxs-lookup"><span data-stu-id="88752-193">b.</span></span> <span data-ttu-id="88752-194">V hello **zprostředkovatele Identity jednotného přihlašování k adrese URL** textového pole, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="88752-194">In hello **Identity provider SSO URL** text box, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="88752-195">c.</span><span class="sxs-lookup"><span data-stu-id="88752-195">c.</span></span> <span data-ttu-id="88752-196">Otevřete certifikát hello stáhli z Azure portal a zkopírujte hello hodnoty bez hello Begin a End řádky a vložte ji do hello **X509 veřejný certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="88752-196">Open hello downloaded certificate from Azure portal and copy hello values without hello Begin and End lines and paste it in hello **Public X509 certificate** box.</span></span>
    
    <span data-ttu-id="88752-197">d.</span><span class="sxs-lookup"><span data-stu-id="88752-197">d.</span></span> <span data-ttu-id="88752-198">Klikněte na tlačítko **uložit konfiguraci** tooSave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="88752-198">Click **Save Configuration**  tooSave hello settings.</span></span>
     
11. <span data-ttu-id="88752-199">Aktualizujte hello Azure AD nastavení toomake že máte hello instalaci opravte identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="88752-199">Update hello Azure AD settings toomake sure that you have setup hello correct Identifier URL.</span></span>
  
    <span data-ttu-id="88752-200">a.</span><span class="sxs-lookup"><span data-stu-id="88752-200">a.</span></span> <span data-ttu-id="88752-201">Kopírování hello **SP Identity ID** z hello SAML obrazovky a vložte ji ve službě Azure AD jako hello **identifikátor** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="88752-201">Copy hello **SP Identity ID** from hello SAML screen and paste it in Azure AD as hello **Identifier** value.</span></span>

    <span data-ttu-id="88752-202">b.</span><span class="sxs-lookup"><span data-stu-id="88752-202">b.</span></span> <span data-ttu-id="88752-203">Přihlašovací adresa URL je adresa URL klienta hello ve vašem cloudu Atlassian.</span><span class="sxs-lookup"><span data-stu-id="88752-203">Sign On URL is hello tenant URL of your Atlassian Cloud.</span></span>     

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. <span data-ttu-id="88752-205">V hello portálu Azure, klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88752-205">In hello Azure portal, Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="88752-207">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="88752-207">You can now read a concise version of these instructions inside hello [Azure  portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="88752-208">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="88752-208">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="88752-209">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="88752-209">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="88752-210">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="88752-210">Creating an Azure AD test user</span></span>
<span data-ttu-id="88752-211">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="88752-211">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="88752-213">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="88752-213">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="88752-214">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="88752-214">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="88752-216">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="88752-216">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="88752-218">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="88752-218">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="88752-220">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="88752-220">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="88752-222">a.</span><span class="sxs-lookup"><span data-stu-id="88752-222">a.</span></span> <span data-ttu-id="88752-223">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="88752-223">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="88752-224">b.</span><span class="sxs-lookup"><span data-stu-id="88752-224">b.</span></span> <span data-ttu-id="88752-225">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="88752-225">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="88752-226">c.</span><span class="sxs-lookup"><span data-stu-id="88752-226">c.</span></span> <span data-ttu-id="88752-227">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="88752-227">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="88752-228">d.</span><span class="sxs-lookup"><span data-stu-id="88752-228">d.</span></span> <span data-ttu-id="88752-229">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="88752-229">Click **Create**.</span></span>
 
### <a name="creating-an-atlassian-cloud-test-user"></a><span data-ttu-id="88752-230">Vytváření testovacího uživatele Atlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="88752-230">Creating an Atlassian Cloud test user</span></span>

<span data-ttu-id="88752-231">Uživatelé toolog tooenable Azure AD v cloudu tooAtlassian, se musí být zřízená do Atlassian cloudu.</span><span class="sxs-lookup"><span data-stu-id="88752-231">tooenable Azure AD users toolog in tooAtlassian Cloud, they must be provisioned into Atlassian Cloud.</span></span>  
<span data-ttu-id="88752-232">V případě cloudu Atlassian zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="88752-232">In case of Atlassian Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="88752-233">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="88752-233">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="88752-234">V části Správa lokality hello, klikněte na tlačítko hello **uživatelé** tlačítko</span><span class="sxs-lookup"><span data-stu-id="88752-234">In hello Site administration section, click hello **Users** button</span></span>

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. <span data-ttu-id="88752-236">Klikněte na tlačítko hello **vytvořit uživatele** tlačítko toocreate uživatele v hello Atlassian cloudu</span><span class="sxs-lookup"><span data-stu-id="88752-236">Click hello **Create User** button toocreate a user in hello Atlassian Cloud</span></span>

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. <span data-ttu-id="88752-238">Zadejte uživatele hello **e-mailová adresa**, **uživatelské jméno**, a **úplný název** a přiřaďte hello přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="88752-238">Enter hello user's **Email address**, **Username**, and **Full Name** and assign hello application access.</span></span> 

    ![Vytvoření uživatele Atlassian cloudu](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. <span data-ttu-id="88752-240">Klikněte na tlačítko **vytvořit uživateli** tlačítko, kterou bude odesílat hello e-mailová pozvánka toohello uživatele a po přijetí hello pozvánku hello uživatel se bude v hello systému aktivní.</span><span class="sxs-lookup"><span data-stu-id="88752-240">Click **Create user** button, it will send hello email invitation toohello user and after accepting hello invitation hello user will be active in hello system.</span></span> 

>[!NOTE] 
><span data-ttu-id="88752-241">Můžete také vytvořit hello hromadné uživatele kliknutím hello **vytvořit hromadné** tlačítka na hello část uživatelé.</span><span class="sxs-lookup"><span data-stu-id="88752-241">You can also create hello bulk users by clicking hello **Bulk Create** button in hello Users section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="88752-242">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="88752-242">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="88752-243">V této části povolíte tak, že udělíte přístup tooAtlassian cloudu Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="88752-243">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAtlassian Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="88752-245">**tooassign tooAtlassian Britta Simon cloudu, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="88752-245">**tooassign Britta Simon tooAtlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="88752-246">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="88752-246">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="88752-248">V seznamu aplikace hello vyberte **Atlassian cloudu**.</span><span class="sxs-lookup"><span data-stu-id="88752-248">In hello applications list, select **Atlassian Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. <span data-ttu-id="88752-250">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="88752-250">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="88752-252">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="88752-252">Click **Add** button.</span></span> <span data-ttu-id="88752-253">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88752-253">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="88752-255">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="88752-255">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="88752-256">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88752-256">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="88752-257">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="88752-257">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="88752-258">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="88752-258">Testing single sign-on</span></span>

<span data-ttu-id="88752-259">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="88752-259">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="88752-260">Po kliknutí na tlačítko hello Atlassian cloudu dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Atlassian cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="88752-260">When you click hello Atlassian Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Atlassian Cloud application.</span></span> <span data-ttu-id="88752-261">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="88752-261">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="88752-262">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="88752-262">Additional resources</span></span>

* [<span data-ttu-id="88752-263">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="88752-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="88752-264">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="88752-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

