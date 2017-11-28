---
title: 'Kurz: Azure Active Directory integrace s Veracode | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Veracode."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d17307b3864b7df8ee55f569d8f962e2e315b936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="611b0-103">Kurz: Azure Active Directory integrace s Veracode</span><span class="sxs-lookup"><span data-stu-id="611b0-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="611b0-104">V tomto kurzu zjistíte, jak toointegrate Veracode s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="611b0-104">In this tutorial, you learn how toointegrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="611b0-105">Integrace Veracode s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="611b0-105">Integrating Veracode with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="611b0-106">Můžete ovládat ve službě Azure AD, který má přístup tooVeracode.</span><span class="sxs-lookup"><span data-stu-id="611b0-106">You can control in Azure AD who has access tooVeracode.</span></span>
- <span data-ttu-id="611b0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooVeracode (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="611b0-107">You can enable your users tooautomatically get signed-on tooVeracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="611b0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="611b0-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="611b0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="611b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="611b0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="611b0-110">Prerequisites</span></span>

<span data-ttu-id="611b0-111">Integrace služby Azure AD s Veracode tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="611b0-111">tooconfigure Azure AD integration with Veracode, you need hello following items:</span></span>

- <span data-ttu-id="611b0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="611b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="611b0-113">Veracode jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="611b0-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="611b0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="611b0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="611b0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="611b0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="611b0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="611b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="611b0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="611b0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="611b0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="611b0-118">Scenario description</span></span>
<span data-ttu-id="611b0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="611b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="611b0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="611b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="611b0-121">Přidat Veracode z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="611b0-121">Add Veracode from hello gallery</span></span>
2. <span data-ttu-id="611b0-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="611b0-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-hello-gallery"></a><span data-ttu-id="611b0-123">Přidat Veracode z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="611b0-123">Add Veracode from hello gallery</span></span>
<span data-ttu-id="611b0-124">tooconfigure hello integrace Veracode do Azure AD, je nutné tooadd Veracode hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="611b0-124">tooconfigure hello integration of Veracode into Azure AD, you need tooadd Veracode from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="611b0-125">**tooadd Veracode z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="611b0-125">**tooadd Veracode from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="611b0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="611b0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="611b0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="611b0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="611b0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="611b0-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="611b0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="611b0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="611b0-133">Hello vyhledávacího pole zadejte **Veracode**, vyberte **Veracode** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="611b0-133">In hello search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Veracode v seznamu výsledků hello](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="611b0-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="611b0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="611b0-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Veracode podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="611b0-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="611b0-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Veracode je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="611b0-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veracode is tooa user in Azure AD.</span></span> <span data-ttu-id="611b0-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Veracode musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="611b0-138">In other words, a link relationship between an Azure AD user and hello related user in Veracode needs toobe established.</span></span>

<span data-ttu-id="611b0-139">V Veracode, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="611b0-139">In Veracode, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="611b0-140">tooconfigure a testu Azure AD jednotné přihlašování s Veracode, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="611b0-140">tooconfigure and test Azure AD single sign-on with Veracode, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="611b0-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="611b0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="611b0-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="611b0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="611b0-143">**[Vytvoření zkušebního uživatele Veracode](#create-a-veracode-test-user)**  -toohave protějšek Britta Simon v Veracode, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="611b0-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - toohave a counterpart of Britta Simon in Veracode that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="611b0-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="611b0-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="611b0-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="611b0-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="611b0-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="611b0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="611b0-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Veracode.</span><span class="sxs-lookup"><span data-stu-id="611b0-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="611b0-148">**tooconfigure Azure AD jednotné přihlašování s Veracode, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="611b0-148">**tooconfigure Azure AD single sign-on with Veracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="611b0-149">V portálu Azure, na hello hello **Veracode** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="611b0-149">In hello Azure portal, on hello **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="611b0-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="611b0-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="611b0-153">Na hello **Veracode domény a adresy URL** části uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="611b0-153">On hello **Veracode Domain and URLs** section, the user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="611b0-155">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="611b0-155">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="611b0-157">Hello cílem této části je toooutline jak tooVeracode tooauthenticate tooenable uživatelé do svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML hello.</span><span class="sxs-lookup"><span data-stu-id="611b0-157">hello objective of this section is toooutline how tooenable users tooauthenticate tooVeracode with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="611b0-158">Aplikace Veracode očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu saml** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="611b0-158">Your Veracode application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> <span data-ttu-id="611b0-159">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="611b0-159">hello following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="611b0-160">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="611b0-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="611b0-161">mapování atributů tooadd hello vyžaduje, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="611b0-161">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="611b0-162">Název atributu</span><span class="sxs-lookup"><span data-stu-id="611b0-162">Attribute Name</span></span> | <span data-ttu-id="611b0-163">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="611b0-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="611b0-164">FirstName</span><span class="sxs-lookup"><span data-stu-id="611b0-164">firstname</span></span> |<span data-ttu-id="611b0-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="611b0-165">User.givenname</span></span> |
    | <span data-ttu-id="611b0-166">Příjmení</span><span class="sxs-lookup"><span data-stu-id="611b0-166">lastname</span></span> |<span data-ttu-id="611b0-167">User.Surname</span><span class="sxs-lookup"><span data-stu-id="611b0-167">User.surname</span></span> |
    | <span data-ttu-id="611b0-168">E-mailu</span><span class="sxs-lookup"><span data-stu-id="611b0-168">email</span></span> |<span data-ttu-id="611b0-169">User.Mail</span><span class="sxs-lookup"><span data-stu-id="611b0-169">User.mail</span></span> |
    
    <span data-ttu-id="611b0-170">a.</span><span class="sxs-lookup"><span data-stu-id="611b0-170">a.</span></span> <span data-ttu-id="611b0-171">Pro každý řádek dat v tabulce hello výše, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="611b0-171">For each data row in hello table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="611b0-172">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="611b0-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="611b0-173">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="611b0-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="611b0-174">b.</span><span class="sxs-lookup"><span data-stu-id="611b0-174">b.</span></span> <span data-ttu-id="611b0-175">V hello **název atributu** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="611b0-175">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="611b0-176">c.</span><span class="sxs-lookup"><span data-stu-id="611b0-176">c.</span></span> <span data-ttu-id="611b0-177">V hello **hodnota atributu** textovému poli, hodnota atributu vyberte hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="611b0-177">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="611b0-178">d.</span><span class="sxs-lookup"><span data-stu-id="611b0-178">d.</span></span> <span data-ttu-id="611b0-179">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="611b0-179">Click **Ok**.</span></span>

7. <span data-ttu-id="611b0-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="611b0-180">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="611b0-182">Na hello **Veracode konfigurace** klikněte na tlačítko **konfigurace Veracode** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="611b0-182">On hello **Veracode Configuration** section, click **Configure Veracode** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="611b0-183">Kopírování hello **SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="611b0-183">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurace Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="611b0-185">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Veracode.</span><span class="sxs-lookup"><span data-stu-id="611b0-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="611b0-186">V nabídce hello hello nahoře, klikněte na tlačítko **nastavení**a potom klikněte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="611b0-186">In hello menu on hello top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="611b0-187">![Správa](./media/active-directory-saas-veracode-tutorial/ic802911.png "správy")</span><span class="sxs-lookup"><span data-stu-id="611b0-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="611b0-188">Klikněte na tlačítko hello **SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="611b0-188">Click hello **SAML** tab.</span></span>

12. <span data-ttu-id="611b0-189">V hello **organizace SAML nastavení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="611b0-189">In hello **Organization SAML Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="611b0-190">![Správa](./media/active-directory-saas-veracode-tutorial/ic802912.png "správy")</span><span class="sxs-lookup"><span data-stu-id="611b0-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="611b0-191">a.</span><span class="sxs-lookup"><span data-stu-id="611b0-191">a.</span></span>  <span data-ttu-id="611b0-192">V **vystavitele** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="611b0-192">In  **Issuer** textbox, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="611b0-193">b.</span><span class="sxs-lookup"><span data-stu-id="611b0-193">b.</span></span> <span data-ttu-id="611b0-194">Klikněte na svůj certifikát stažený z portálu Azure, tooupload **zvolit soubor**.</span><span class="sxs-lookup"><span data-stu-id="611b0-194">tooupload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="611b0-195">c.</span><span class="sxs-lookup"><span data-stu-id="611b0-195">c.</span></span> <span data-ttu-id="611b0-196">Vyberte **povolit samoobslužné registrace**.</span><span class="sxs-lookup"><span data-stu-id="611b0-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="611b0-197">V hello **nastavení samoobslužné registrace** část, proveďte hello následující kroky a pak klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="611b0-197">In hello **Self Registration Settings** section, perform hello following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="611b0-198">![Správa](./media/active-directory-saas-veracode-tutorial/ic802913.png "správy")</span><span class="sxs-lookup"><span data-stu-id="611b0-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="611b0-199">a.</span><span class="sxs-lookup"><span data-stu-id="611b0-199">a.</span></span> <span data-ttu-id="611b0-200">Jako **nové aktivace uživatele**, vyberte **vyžaduje její aktivace ne**.</span><span class="sxs-lookup"><span data-stu-id="611b0-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="611b0-201">b.</span><span class="sxs-lookup"><span data-stu-id="611b0-201">b.</span></span> <span data-ttu-id="611b0-202">Jako **aktualizace dat uživatele**, vyberte **předvoleb Veracode uživatelská Data**.</span><span class="sxs-lookup"><span data-stu-id="611b0-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="611b0-203">c.</span><span class="sxs-lookup"><span data-stu-id="611b0-203">c.</span></span> <span data-ttu-id="611b0-204">Pro **SAML atribut podrobnosti**, vyberte hello následující:</span><span class="sxs-lookup"><span data-stu-id="611b0-204">For **SAML Attribute Details**, select hello following:</span></span>
      * <span data-ttu-id="611b0-205">**Role uživatele**</span><span class="sxs-lookup"><span data-stu-id="611b0-205">**User Roles**</span></span>
      * <span data-ttu-id="611b0-206">**Správce zásad**</span><span class="sxs-lookup"><span data-stu-id="611b0-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="611b0-207">**Kontrolora**</span><span class="sxs-lookup"><span data-stu-id="611b0-207">**Reviewer**</span></span>
      * <span data-ttu-id="611b0-208">**Realizace zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="611b0-208">**Security Lead**</span></span>
      * <span data-ttu-id="611b0-209">**Vedení**</span><span class="sxs-lookup"><span data-stu-id="611b0-209">**Executive**</span></span>
      * <span data-ttu-id="611b0-210">**Odesílatel**</span><span class="sxs-lookup"><span data-stu-id="611b0-210">**Submitter**</span></span>
      * <span data-ttu-id="611b0-211">**Tvůrce**</span><span class="sxs-lookup"><span data-stu-id="611b0-211">**Creator**</span></span>
      * <span data-ttu-id="611b0-212">**Všechny kontroly typy**</span><span class="sxs-lookup"><span data-stu-id="611b0-212">**All Scan Types**</span></span>
      * <span data-ttu-id="611b0-213">**Členství v týmu**</span><span class="sxs-lookup"><span data-stu-id="611b0-213">**Team Memberships**</span></span>
      * <span data-ttu-id="611b0-214">**Výchozí tým**</span><span class="sxs-lookup"><span data-stu-id="611b0-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="611b0-215">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="611b0-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="611b0-216">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="611b0-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="611b0-217">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="611b0-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="611b0-218">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="611b0-218">Create an Azure AD test user</span></span>

<span data-ttu-id="611b0-219">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="611b0-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="611b0-221">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="611b0-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="611b0-222">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="611b0-222">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="611b0-224">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="611b0-224">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="611b0-226">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="611b0-226">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="611b0-228">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="611b0-228">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="611b0-230">a.</span><span class="sxs-lookup"><span data-stu-id="611b0-230">a.</span></span> <span data-ttu-id="611b0-231">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="611b0-231">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="611b0-232">b.</span><span class="sxs-lookup"><span data-stu-id="611b0-232">b.</span></span> <span data-ttu-id="611b0-233">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="611b0-233">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="611b0-234">c.</span><span class="sxs-lookup"><span data-stu-id="611b0-234">c.</span></span> <span data-ttu-id="611b0-235">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="611b0-235">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="611b0-236">d.</span><span class="sxs-lookup"><span data-stu-id="611b0-236">d.</span></span> <span data-ttu-id="611b0-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="611b0-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="611b0-238">Vytvoření zkušebního uživatele Veracode</span><span class="sxs-lookup"><span data-stu-id="611b0-238">Create a Veracode test user</span></span>
<span data-ttu-id="611b0-239">V pořadí tooenable Azure AD Uživatelé toolog do Veracode musí být zřízená do Veracode.</span><span class="sxs-lookup"><span data-stu-id="611b0-239">In order tooenable Azure AD users toolog into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="611b0-240">V případě hello Veracode zřizování je automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="611b0-240">In hello case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="611b0-241">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="611b0-241">There is no action item for you.</span></span> <span data-ttu-id="611b0-242">Uživatelé jsou automaticky vytvářeny v případě potřeby během hello první jeden přihlašování pokus.</span><span class="sxs-lookup"><span data-stu-id="611b0-242">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="611b0-243">Můžete použít všechny ostatní Veracode uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Veracode tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="611b0-243">You can use any other Veracode user account creation tools or APIs provided by Veracode tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="611b0-244">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="611b0-244">Assign hello Azure AD test user</span></span>

<span data-ttu-id="611b0-245">V této části povolíte tak, že udělíte přístup tooVeracode toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="611b0-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeracode.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="611b0-247">**tooassign Britta Simon tooVeracode, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="611b0-247">**tooassign Britta Simon tooVeracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="611b0-248">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="611b0-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="611b0-250">V seznamu aplikace hello vyberte **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="611b0-250">In hello applications list, select **Veracode**.</span></span>

    ![v seznamu aplikace hello Hello Veracode odkaz](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="611b0-252">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="611b0-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="611b0-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="611b0-254">Click **Add** button.</span></span> <span data-ttu-id="611b0-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="611b0-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="611b0-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="611b0-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="611b0-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="611b0-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="611b0-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="611b0-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="611b0-260">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="611b0-260">Test single sign-on</span></span>

<span data-ttu-id="611b0-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="611b0-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="611b0-262">Když kliknete na dlaždici Veracode hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Veracode aplikace.</span><span class="sxs-lookup"><span data-stu-id="611b0-262">When you click hello Veracode tile in hello Access Panel, you should get automatically signed-on tooyour Veracode application.</span></span>
<span data-ttu-id="611b0-263">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="611b0-263">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="611b0-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="611b0-264">Additional resources</span></span>

* [<span data-ttu-id="611b0-265">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="611b0-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="611b0-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="611b0-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

