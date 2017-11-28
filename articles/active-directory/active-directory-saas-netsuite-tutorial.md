---
title: 'Kurz: Azure Active Directory integrace s Netsuite | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="75f34-103">Kurz: Azure Active Directory integrace s Netsuite</span><span class="sxs-lookup"><span data-stu-id="75f34-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="75f34-104">V tomto kurzu zjistíte, jak toointegrate Netsuite s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="75f34-104">In this tutorial, you learn how toointegrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75f34-105">Integrace Netsuite s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="75f34-105">Integrating Netsuite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75f34-106">Můžete řídit ve službě Azure AD, který má přístup tooNetsuite</span><span class="sxs-lookup"><span data-stu-id="75f34-106">You can control in Azure AD who has access tooNetsuite</span></span>
- <span data-ttu-id="75f34-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNetsuite (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="75f34-107">You can enable your users tooautomatically get signed-on tooNetsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75f34-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="75f34-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="75f34-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75f34-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75f34-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="75f34-110">Prerequisites</span></span>

<span data-ttu-id="75f34-111">Integrace služby Azure AD s Netsuite tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="75f34-111">tooconfigure Azure AD integration with Netsuite, you need hello following items:</span></span>

- <span data-ttu-id="75f34-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="75f34-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75f34-113">Netsuite jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="75f34-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75f34-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="75f34-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75f34-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="75f34-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75f34-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="75f34-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75f34-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75f34-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75f34-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="75f34-118">Scenario description</span></span>
<span data-ttu-id="75f34-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="75f34-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75f34-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="75f34-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75f34-121">Přidání Netsuite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="75f34-121">Adding Netsuite from hello gallery</span></span>
2. <span data-ttu-id="75f34-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="75f34-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-hello-gallery"></a><span data-ttu-id="75f34-123">Přidání Netsuite z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="75f34-123">Adding Netsuite from hello gallery</span></span>
<span data-ttu-id="75f34-124">tooconfigure hello integrace Netsuite do Azure AD, je nutné tooadd Netsuite hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="75f34-124">tooconfigure hello integration of Netsuite into Azure AD, you need tooadd Netsuite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75f34-125">**tooadd Netsuite z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75f34-125">**tooadd Netsuite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75f34-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="75f34-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75f34-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="75f34-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75f34-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="75f34-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75f34-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="75f34-133">Hello vyhledávacího pole zadejte **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="75f34-133">In hello search box, type **Netsuite**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="75f34-135">Na panelu výsledků hello vyberte **Netsuite**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="75f34-135">In hello results panel, select **Netsuite**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75f34-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="75f34-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75f34-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Netsuite podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="75f34-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="75f34-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Netsuite je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75f34-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Netsuite is tooa user in Azure AD.</span></span> <span data-ttu-id="75f34-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Netsuite musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="75f34-140">In other words, a link relationship between an Azure AD user and hello related user in Netsuite needs toobe established.</span></span>

<span data-ttu-id="75f34-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Netsuite.</span><span class="sxs-lookup"><span data-stu-id="75f34-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Netsuite.</span></span>

<span data-ttu-id="75f34-142">tooconfigure a testu Azure AD jednotné přihlašování s Netsuite, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="75f34-142">tooconfigure and test Azure AD single sign-on with Netsuite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75f34-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="75f34-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75f34-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75f34-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75f34-145">**[Vytvoření zkušebního uživatele Netsuite](#creating-a-netsuite-test-user)**  -toohave protějšek Britta Simon v Netsuite, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="75f34-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - toohave a counterpart of Britta Simon in Netsuite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="75f34-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75f34-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75f34-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="75f34-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75f34-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="75f34-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75f34-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Netsuite.</span><span class="sxs-lookup"><span data-stu-id="75f34-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="75f34-150">**tooconfigure Azure AD jednotné přihlašování s Netsuite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75f34-150">**tooconfigure Azure AD single sign-on with Netsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="75f34-151">V portálu Azure, na hello hello **Netsuite** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="75f34-151">In hello Azure portal, on hello **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="75f34-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75f34-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="75f34-155">Na hello **Netsuite domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="75f34-155">On hello **Netsuite Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="75f34-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzor: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="75f34-157">In hello **Reply URL** textbox, type a URL using hello following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="75f34-158">Tato hodnota není skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="75f34-158">This value is not real value.</span></span> <span data-ttu-id="75f34-159">Aktualizace hello hodnotu s hello skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="75f34-159">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="75f34-160">Obraťte se na [tým podpory Netsuite](http://www.netsuite.com/portal/services/support.shtml) tooget tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="75f34-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) tooget this value.</span></span>
 
4. <span data-ttu-id="75f34-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="75f34-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="75f34-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75f34-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75f34-165">Na hello **Netsuite konfigurace** klikněte na tlačítko **konfigurace Netsuite** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="75f34-165">On hello **Netsuite Configuration** section, click **Configure Netsuite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="75f34-166">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="75f34-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="75f34-168">V prohlížeči otevřete novou kartu a přihlaste se k serveru vaší společnosti Netsuite jako správce.</span><span class="sxs-lookup"><span data-stu-id="75f34-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="75f34-169">Na hello nástrojů v horní části hello hello stránky, klikněte na **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-169">In hello toolbar at hello top of hello page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="75f34-171">Z hello **úlohy nastavení** seznamu, vyberte **integrace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-171">From hello **Setup Tasks** list, select **Integration**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="75f34-173">V hello **spravovat ověřování** klikněte na tlačítko **SAML jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="75f34-173">In hello **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="75f34-175">Na hello **SAML instalace** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="75f34-175">On hello **SAML Setup** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="75f34-176">a.</span><span class="sxs-lookup"><span data-stu-id="75f34-176">a.</span></span> <span data-ttu-id="75f34-177">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hodnoty **Stručná referenční příručka** části **konfigurovat přihlášení** a vložte jej do hello **zprostředkovatele Identity Přihlašovací stránka** pole Netsuite.</span><span class="sxs-lookup"><span data-stu-id="75f34-177">Copy hello **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into hello **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="75f34-179">b.</span><span class="sxs-lookup"><span data-stu-id="75f34-179">b.</span></span> <span data-ttu-id="75f34-180">V Netsuite, vyberte **primární metoda ověřování**.</span><span class="sxs-lookup"><span data-stu-id="75f34-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="75f34-181">c.</span><span class="sxs-lookup"><span data-stu-id="75f34-181">c.</span></span> <span data-ttu-id="75f34-182">Pro hello pole s názvem bez přípony **metadat zprostředkovatelů Identity SAMLV2**, vyberte **nahrát soubor metadat IDP**.</span><span class="sxs-lookup"><span data-stu-id="75f34-182">For hello field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="75f34-183">Pak klikněte na tlačítko **Procházet** tooupload hello metadata souboru, který jste stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="75f34-183">Then click **Browse** tooupload hello metadata file that you downloaded from Azure portal.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="75f34-185">d.</span><span class="sxs-lookup"><span data-stu-id="75f34-185">d.</span></span> <span data-ttu-id="75f34-186">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="75f34-186">Click **Submit**.</span></span>

12. <span data-ttu-id="75f34-187">Ve službě Azure AD, klikněte na **zobrazit a upravit všechny ostatní atributy uživatele** zaškrtávací políčko a přidání atributu.</span><span class="sxs-lookup"><span data-stu-id="75f34-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="75f34-189">Pro hello **název atributu** pole, zadejte v `account`.</span><span class="sxs-lookup"><span data-stu-id="75f34-189">For hello **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="75f34-190">Pro hello **hodnota atributu** pole, zadejte v ID Netsuite účtu. Tato hodnota je konstanta a změňte pomocí účtu.</span><span class="sxs-lookup"><span data-stu-id="75f34-190">For hello **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="75f34-191">Pokyny, jak toofind vaše ID pro účet jsou popsány níže:</span><span class="sxs-lookup"><span data-stu-id="75f34-191">Instructions on how toofind your account ID are included below:</span></span>

      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="75f34-193">a.</span><span class="sxs-lookup"><span data-stu-id="75f34-193">a.</span></span> <span data-ttu-id="75f34-194">V Netsuite, klikněte na **instalace** nabídce hello v horním navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="75f34-194">In Netsuite, click **Setup** from hello top navigation menu.</span></span>

    <span data-ttu-id="75f34-195">b.</span><span class="sxs-lookup"><span data-stu-id="75f34-195">b.</span></span> <span data-ttu-id="75f34-196">Klikněte v části hello **úlohy nastavení** části hello levé navigační nabídce vyberte hello **integrace** a klikněte na **webové služby Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="75f34-196">Then click under hello **Setup Tasks** section of hello left navigation menu, select hello **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="75f34-197">c.</span><span class="sxs-lookup"><span data-stu-id="75f34-197">c.</span></span> <span data-ttu-id="75f34-198">Vaše ID pro účet Netsuite zkopírujte a vložte ho do hello **hodnota atributu** pole ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75f34-198">Copy your Netsuite Account ID and paste it into hello **Attribute Value** field in Azure AD.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="75f34-200">Předtím, než mohou uživatelé provádět jednotné přihlašování do Netsuite, je třeba nejprve je přiřadit hello v Netsuite příslušná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="75f34-200">Before users can perform single sign-on into Netsuite, they must first be assigned hello appropriate permissions in Netsuite.</span></span> <span data-ttu-id="75f34-201">Postupujte podle pokynů hello níže tooassign tato oprávnění.</span><span class="sxs-lookup"><span data-stu-id="75f34-201">Follow hello instructions below tooassign these permissions.</span></span>

    <span data-ttu-id="75f34-202">a.</span><span class="sxs-lookup"><span data-stu-id="75f34-202">a.</span></span> <span data-ttu-id="75f34-203">V nabídce hello horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-203">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="75f34-205">b.</span><span class="sxs-lookup"><span data-stu-id="75f34-205">b.</span></span> <span data-ttu-id="75f34-206">V levém navigačním nabídce hello vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat role**.</span><span class="sxs-lookup"><span data-stu-id="75f34-206">On hello left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="75f34-208">c.</span><span class="sxs-lookup"><span data-stu-id="75f34-208">c.</span></span> <span data-ttu-id="75f34-209">Klikněte na tlačítko **novou roli**.</span><span class="sxs-lookup"><span data-stu-id="75f34-209">Click **New Role**.</span></span>

    <span data-ttu-id="75f34-210">d.</span><span class="sxs-lookup"><span data-stu-id="75f34-210">d.</span></span> <span data-ttu-id="75f34-211">Zadejte **název** pro novou roli a vyberte hello **jednoho přihlášení pouze** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="75f34-211">Type in a **Name** for your new role, and select hello **Single Sign-On Only** checkbox.</span></span>
      
      ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="75f34-213">e.</span><span class="sxs-lookup"><span data-stu-id="75f34-213">e.</span></span> <span data-ttu-id="75f34-214">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75f34-214">Click **Save**.</span></span>

    <span data-ttu-id="75f34-215">f.</span><span class="sxs-lookup"><span data-stu-id="75f34-215">f.</span></span> <span data-ttu-id="75f34-216">V nabídce hello hello nahoře, klikněte na tlačítko **oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="75f34-216">In hello menu on hello top, click **Permissions**.</span></span> <span data-ttu-id="75f34-217">Pak klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="75f34-217">Then click **Setup**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="75f34-219">g.</span><span class="sxs-lookup"><span data-stu-id="75f34-219">g.</span></span> <span data-ttu-id="75f34-220">Vyberte **nastavit až SAM jednotné přihlašování**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="75f34-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="75f34-221">h.</span><span class="sxs-lookup"><span data-stu-id="75f34-221">h.</span></span> <span data-ttu-id="75f34-222">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75f34-222">Click **Save**.</span></span>

    <span data-ttu-id="75f34-223">i.</span><span class="sxs-lookup"><span data-stu-id="75f34-223">i.</span></span> <span data-ttu-id="75f34-224">V nabídce hello horním navigačním panelu klikněte na tlačítko **instalace**, pak klikněte na tlačítko **Správce instalace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-224">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="75f34-226">j.</span><span class="sxs-lookup"><span data-stu-id="75f34-226">j.</span></span> <span data-ttu-id="75f34-227">V levém navigačním nabídce hello vyberte **uživatele/role**, pak klikněte na tlačítko **spravovat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="75f34-227">On hello left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="75f34-229">kB.</span><span class="sxs-lookup"><span data-stu-id="75f34-229">k.</span></span> <span data-ttu-id="75f34-230">Vyberte testovací uživatele.</span><span class="sxs-lookup"><span data-stu-id="75f34-230">Select a test user.</span></span> <span data-ttu-id="75f34-231">Pak klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="75f34-231">Then click **Edit**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="75f34-233">l.</span><span class="sxs-lookup"><span data-stu-id="75f34-233">l.</span></span> <span data-ttu-id="75f34-234">V dialogovém okně hello rolí, vyberte roli hello, který jste vytvořili a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="75f34-234">On hello Roles dialog, select hello role that you have created and click **Add**.</span></span>
      
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="75f34-236">m.</span><span class="sxs-lookup"><span data-stu-id="75f34-236">m.</span></span> <span data-ttu-id="75f34-237">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="75f34-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="75f34-238">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="75f34-238">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="75f34-239">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="75f34-239">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="75f34-240">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75f34-240">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75f34-241">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="75f34-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="75f34-242">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="75f34-242">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="75f34-244">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75f34-244">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75f34-245">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="75f34-245">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="75f34-247">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="75f34-247">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75f34-249">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75f34-249">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75f34-251">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="75f34-251">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75f34-253">a.</span><span class="sxs-lookup"><span data-stu-id="75f34-253">a.</span></span> <span data-ttu-id="75f34-254">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75f34-254">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75f34-255">b.</span><span class="sxs-lookup"><span data-stu-id="75f34-255">b.</span></span> <span data-ttu-id="75f34-256">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75f34-256">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75f34-257">c.</span><span class="sxs-lookup"><span data-stu-id="75f34-257">c.</span></span> <span data-ttu-id="75f34-258">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="75f34-258">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="75f34-259">d.</span><span class="sxs-lookup"><span data-stu-id="75f34-259">d.</span></span> <span data-ttu-id="75f34-260">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="75f34-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="75f34-261">Vytvoření zkušebního uživatele Netsuite</span><span class="sxs-lookup"><span data-stu-id="75f34-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="75f34-262">V této části se uživatel volá Britta Simon vytvoří v Netsuite.</span><span class="sxs-lookup"><span data-stu-id="75f34-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="75f34-263">Netsuite podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="75f34-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="75f34-264">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="75f34-264">There is no action item for you in this section.</span></span> <span data-ttu-id="75f34-265">Pokud uživatel v Netsuite ještě neexistuje, je vytvořen nový pokusíte-li tooaccess Netsuite.</span><span class="sxs-lookup"><span data-stu-id="75f34-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt tooaccess Netsuite.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="75f34-266">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="75f34-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="75f34-267">V této části povolíte tak, že udělíte přístup tooNetsuite toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="75f34-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNetsuite.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="75f34-269">**tooassign Britta Simon tooNetsuite, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="75f34-269">**tooassign Britta Simon tooNetsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="75f34-270">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="75f34-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="75f34-272">V seznamu aplikace hello vyberte **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="75f34-272">In hello applications list, select **Netsuite**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="75f34-274">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="75f34-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="75f34-276">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="75f34-276">Click **Add** button.</span></span> <span data-ttu-id="75f34-277">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75f34-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="75f34-279">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="75f34-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75f34-280">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75f34-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75f34-281">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="75f34-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="75f34-282">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="75f34-282">Testing single sign-on</span></span>

<span data-ttu-id="75f34-283">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="75f34-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="75f34-284">tootest jeden přihlašování nastavení, otevřete hello přístupovému panelu na adrese [https://myapps.microsoft.com](https://myapps.microsoft.com/), přihlaste se k hello testovací účet a klikněte na tlačítko **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="75f34-284">tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into hello test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75f34-285">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="75f34-285">Additional resources</span></span>

* [<span data-ttu-id="75f34-286">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75f34-286">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75f34-287">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75f34-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="75f34-288">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="75f34-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

