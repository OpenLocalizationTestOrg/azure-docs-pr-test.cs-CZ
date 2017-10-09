---
title: 'Kurz: Azure Active Directory integrace s Trello | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Trello."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cd5ae365-9ed6-43a6-920b-f7814b993949
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: de2f2ba6a0e5545983c351f26f99d14f436618c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trello"></a><span data-ttu-id="6a792-103">Kurz: Azure Active Directory integrace s Trello</span><span class="sxs-lookup"><span data-stu-id="6a792-103">Tutorial: Azure Active Directory integration with Trello</span></span>

<span data-ttu-id="6a792-104">V tomto kurzu zjistíte, jak toointegrate Trello s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6a792-104">In this tutorial, you learn how toointegrate Trello with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6a792-105">Integrace Trello s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6a792-105">Integrating Trello with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6a792-106">Můžete řídit ve službě Azure AD, který má přístup tooTrello</span><span class="sxs-lookup"><span data-stu-id="6a792-106">You can control in Azure AD who has access tooTrello</span></span>
- <span data-ttu-id="6a792-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTrello (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a792-107">You can enable your users tooautomatically get signed-on tooTrello (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6a792-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6a792-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6a792-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6a792-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a792-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6a792-110">Prerequisites</span></span>

<span data-ttu-id="6a792-111">Integrace služby Azure AD s Trello tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6a792-111">tooconfigure Azure AD integration with Trello, you need hello following items:</span></span>

- <span data-ttu-id="6a792-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a792-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6a792-113">Trello jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6a792-113">A Trello single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6a792-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a792-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6a792-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6a792-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6a792-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6a792-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6a792-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6a792-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6a792-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6a792-118">Scenario description</span></span>
<span data-ttu-id="6a792-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a792-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6a792-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6a792-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6a792-121">Přidání Trello z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a792-121">Adding Trello from hello gallery</span></span>
2. <span data-ttu-id="6a792-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a792-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trello-from-hello-gallery"></a><span data-ttu-id="6a792-123">Přidání Trello z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6a792-123">Adding Trello from hello gallery</span></span>
<span data-ttu-id="6a792-124">tooconfigure hello integrace Trello do Azure AD, je nutné tooadd Trello hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6a792-124">tooconfigure hello integration of Trello into Azure AD, you need tooadd Trello from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6a792-125">**tooadd Trello z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a792-125">**tooadd Trello from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a792-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a792-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6a792-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6a792-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6a792-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a792-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6a792-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a792-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6a792-133">Hello vyhledávacího pole zadejte **Trello**.</span><span class="sxs-lookup"><span data-stu-id="6a792-133">In hello search box, type **Trello**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_search.png)

5. <span data-ttu-id="6a792-135">Na panelu výsledků hello vyberte **Trello**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a792-135">In hello results panel, select **Trello**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/tutorial_trello_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6a792-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a792-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6a792-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Trello podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6a792-138">In this section, you configure and test Azure AD single sign-on with Trello based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6a792-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Trello je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a792-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Trello is tooa user in Azure AD.</span></span> <span data-ttu-id="6a792-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Trello musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6a792-140">In other words, a link relationship between an Azure AD user and hello related user in Trello needs toobe established.</span></span>

<span data-ttu-id="6a792-141">V Trello, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6a792-141">In Trello, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6a792-142">tooconfigure a testu Azure AD jednotné přihlašování s Trello, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6a792-142">tooconfigure and test Azure AD single sign-on with Trello, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6a792-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6a792-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6a792-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6a792-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6a792-145">**[Vytvoření zkušebního uživatele Trello](#creating-a-trello-test-user)**  -toohave protějšek Britta Simon v Trello, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a792-145">**[Creating a Trello test user](#creating-a-trello-test-user)** - toohave a counterpart of Britta Simon in Trello that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6a792-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a792-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6a792-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6a792-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6a792-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a792-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6a792-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Trello.</span><span class="sxs-lookup"><span data-stu-id="6a792-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Trello application.</span></span>

<span data-ttu-id="6a792-150">**tooconfigure Azure AD jednotné přihlašování s Trello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a792-150">**tooconfigure Azure AD single sign-on with Trello, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a792-151">V portálu Azure, na hello hello **Trello** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6a792-151">In hello Azure portal, on hello **Trello** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6a792-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a792-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_samlbase.png)

3. <span data-ttu-id="6a792-155">Na hello **Trello domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6a792-155">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_url.png)

    <span data-ttu-id="6a792-157">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="6a792-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

4. <span data-ttu-id="6a792-158">Na hello **Trello domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **SP iniciované režimu**, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6a792-158">On hello **Trello Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_url1.png)

    <span data-ttu-id="6a792-160">a.</span><span class="sxs-lookup"><span data-stu-id="6a792-160">a.</span></span> <span data-ttu-id="6a792-161">Klikněte na hello **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="6a792-161">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="6a792-162">b.</span><span class="sxs-lookup"><span data-stu-id="6a792-162">b.</span></span> <span data-ttu-id="6a792-163">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://trello.com/auth/saml/consume/<enterprise>`</span><span class="sxs-lookup"><span data-stu-id="6a792-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://trello.com/auth/saml/consume/<enterprise>`</span></span>

    >[!NOTE]
    ><span data-ttu-id="6a792-164">Měli byste obdržet hello  **\<enterprise\>**  zkráceného názvu stránky z Trello.</span><span class="sxs-lookup"><span data-stu-id="6a792-164">You should get hello **\<enterprise\>** slug from Trello.</span></span> <span data-ttu-id="6a792-165">Pokud nemáte hodnotu hello zkráceného názvu stránky, obraťte se na [tým podpory Trello](mailto:support@trello.com) tooget hello zkráceného názvu stránky pro můžete enterprise.</span><span class="sxs-lookup"><span data-stu-id="6a792-165">If you don't have hello slug value, contact [Trello support team](mailto:support@trello.com) tooget hello slug for you enterprise.</span></span>
    > 

5. <span data-ttu-id="6a792-166">Aplikace Trello očekává hello SAML kontrolní výrazy toocontain konkrétní atributy.</span><span class="sxs-lookup"><span data-stu-id="6a792-166">Trello application expects hello SAML assertions toocontain specific attributes.</span></span> <span data-ttu-id="6a792-167">Nakonfigurujte hello následující atributy pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a792-167">Configure hello following attributes  for this application.</span></span> <span data-ttu-id="6a792-168">Můžete spravovat hello hodnoty těchto atributů z hello **"Atributy uživatele"** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6a792-168">You can manage hello values of these attributes from hello **"User Attributes"** of hello application.</span></span> <span data-ttu-id="6a792-169">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="6a792-169">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_attribute.png)

6. <span data-ttu-id="6a792-171">Na hello **atributy tokenu SAML** dialogu pro každý řádek v tabulce hello níže, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a792-171">On hello **SAML token attributes** dialog, for each row shown in hello table below, perform hello following steps:</span></span>
 
    | <span data-ttu-id="6a792-172">Název atributu</span><span class="sxs-lookup"><span data-stu-id="6a792-172">Attribute Name</span></span> | <span data-ttu-id="6a792-173">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="6a792-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="6a792-174">User.Email</span><span class="sxs-lookup"><span data-stu-id="6a792-174">User.Email</span></span> | <span data-ttu-id="6a792-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="6a792-175">user.mail</span></span> |
    | <span data-ttu-id="6a792-176">User.FirstName</span><span class="sxs-lookup"><span data-stu-id="6a792-176">User.FirstName</span></span> | <span data-ttu-id="6a792-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6a792-177">user.givenname</span></span> |
    | <span data-ttu-id="6a792-178">User.LastName</span><span class="sxs-lookup"><span data-stu-id="6a792-178">User.LastName</span></span> | <span data-ttu-id="6a792-179">User.Surname</span><span class="sxs-lookup"><span data-stu-id="6a792-179">user.surname</span></span> |

    <span data-ttu-id="6a792-180">a.</span><span class="sxs-lookup"><span data-stu-id="6a792-180">a.</span></span> <span data-ttu-id="6a792-181">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a792-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="6a792-184">b.</span><span class="sxs-lookup"><span data-stu-id="6a792-184">b.</span></span> <span data-ttu-id="6a792-185">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6a792-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span> 

    <span data-ttu-id="6a792-186">c.</span><span class="sxs-lookup"><span data-stu-id="6a792-186">c.</span></span> <span data-ttu-id="6a792-187">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="6a792-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6a792-188">d.</span><span class="sxs-lookup"><span data-stu-id="6a792-188">d.</span></span> <span data-ttu-id="6a792-189">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a792-189">Click **Ok**.</span></span> 
 
7. <span data-ttu-id="6a792-190">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6a792-190">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_certificate.png) 

8. <span data-ttu-id="6a792-192">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a792-192">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6a792-194">Na hello **Trello konfigurace** klikněte na tlačítko **konfigurace Trello** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6a792-194">On hello **Trello Configuration** section, click **Configure Trello** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6a792-195">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6a792-195">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_configure.png) 

9. <span data-ttu-id="6a792-197">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte příliš[Konfigurace jednotného přihlašování k enterprise Trello](https://trello.com/sso-configuration) stránky toosend [tým podpory Trello](mailto:support@trello.com) hello **SAML jeden přihlašování adresa URL služby** a Připojte hello **certifikátu (Base64)**.</span><span class="sxs-lookup"><span data-stu-id="6a792-197">tooget SSO configured for your application, go too[Trello enterprise SSO configuration](https://trello.com/sso-configuration) page toosend [Trello support team](mailto:support@trello.com) hello **SAML Single Sign-On Service URL** and attach hello **Certificate (Base64)**.</span></span>

> [!TIP]
> <span data-ttu-id="6a792-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6a792-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6a792-199">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6a792-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6a792-200">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6a792-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6a792-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6a792-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="6a792-202">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6a792-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6a792-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a792-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a792-205">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6a792-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6a792-207">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6a792-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6a792-209">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6a792-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6a792-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a792-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6a792-213">a.</span><span class="sxs-lookup"><span data-stu-id="6a792-213">a.</span></span> <span data-ttu-id="6a792-214">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6a792-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6a792-215">b.</span><span class="sxs-lookup"><span data-stu-id="6a792-215">b.</span></span> <span data-ttu-id="6a792-216">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6a792-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6a792-217">c.</span><span class="sxs-lookup"><span data-stu-id="6a792-217">c.</span></span> <span data-ttu-id="6a792-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6a792-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6a792-219">d.</span><span class="sxs-lookup"><span data-stu-id="6a792-219">d.</span></span> <span data-ttu-id="6a792-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a792-220">Click **Create**.</span></span>
 
### <a name="creating-a-trello-test-user"></a><span data-ttu-id="6a792-221">Vytvoření zkušebního uživatele Trello</span><span class="sxs-lookup"><span data-stu-id="6a792-221">Creating a Trello test user</span></span>

<span data-ttu-id="6a792-222">V této části vytvoříte volal Britta Simon v Trello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a792-222">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="6a792-223">V této části vytvoříte volal Britta Simon v Trello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a792-223">In this section, you create a user called Britta Simon in Trello.</span></span> <span data-ttu-id="6a792-224">Trello podporuje zřizování za běhu a nový účet je vytvořen hello při prvním přihlášení z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6a792-224">Trello supports just-in-time provisioning and a new account is created hello first time you sign in from Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6a792-225">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6a792-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6a792-226">V této části povolíte tak, že udělíte přístup tooTrello toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6a792-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTrello.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6a792-228">**tooassign Britta Simon tooTrello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6a792-228">**tooassign Britta Simon tooTrello, perform hello following steps:**</span></span>

1. <span data-ttu-id="6a792-229">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6a792-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6a792-231">V seznamu aplikace hello vyberte **Trello**.</span><span class="sxs-lookup"><span data-stu-id="6a792-231">In hello applications list, select **Trello**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-trello-tutorial/tutorial_trello_app.png) 

3. <span data-ttu-id="6a792-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6a792-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6a792-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a792-235">Click **Add** button.</span></span> <span data-ttu-id="6a792-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a792-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6a792-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6a792-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6a792-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a792-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6a792-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6a792-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6a792-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6a792-241">Testing single sign-on</span></span>

<span data-ttu-id="6a792-242">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="6a792-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6a792-243">Když kliknete na dlaždici Trello hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Trello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a792-243">When you click hello Trello tile in hello Access Panel, you should get automatically signed-on tooyour Trello application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a792-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6a792-244">Additional resources</span></span>

* [<span data-ttu-id="6a792-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6a792-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6a792-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6a792-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trello-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png

