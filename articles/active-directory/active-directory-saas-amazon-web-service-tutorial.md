---
title: 'Kurz: Azure Active Directory integrace s Amazon Web Services (AWS) | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="2c761-103">Kurz: Azure Active Directory integrace s Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="2c761-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="2c761-104">V tomto kurzu zjistíte, jak toointegrate Amazon Web Services (AWS) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c761-104">In this tutorial, you learn how toointegrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c761-105">Integrace Amazon Web Services (AWS) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2c761-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2c761-106">Můžete řídit ve službě Azure AD, který má přístup tooAmazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="2c761-106">You can control in Azure AD who has access tooAmazon Web Services (AWS)</span></span>
- <span data-ttu-id="2c761-107">Vaši uživatelé tooautomatically get přihlášeného tooAmazon Web Services (AWS) (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c761-107">You can enable your users tooautomatically get signed-on tooAmazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c761-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2c761-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2c761-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c761-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="2c761-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c761-110">Prerequisites</span></span>

<span data-ttu-id="2c761-111">tooconfigure integrace Azure AD pomocí Amazon Web Services (AWS), je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2c761-111">tooconfigure Azure AD integration with Amazon Web Services (AWS), you need hello following items:</span></span>

- <span data-ttu-id="2c761-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c761-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c761-113">Amazon Web Services (AWS) jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2c761-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2c761-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c761-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2c761-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2c761-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c761-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="2c761-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2c761-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c761-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c761-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2c761-118">Scenario description</span></span>
<span data-ttu-id="2c761-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c761-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2c761-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2c761-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c761-121">Přidání Amazon Web Services (AWS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c761-121">Adding Amazon Web Services (AWS) from hello gallery</span></span>
2. <span data-ttu-id="2c761-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c761-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a><span data-ttu-id="2c761-123">Přidání Amazon Web Services (AWS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2c761-123">Adding Amazon Web Services (AWS) from hello gallery</span></span>
<span data-ttu-id="2c761-124">tooconfigure hello integrace Amazon Web Services (AWS) do Azure AD, je nutné tooadd Amazon Web Services (AWS) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2c761-124">tooconfigure hello integration of Amazon Web Services (AWS) into Azure AD, you need tooadd Amazon Web Services (AWS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2c761-125">**tooadd Amazon Web Services (AWS) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c761-125">**tooadd Amazon Web Services (AWS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c761-126">V hello  **[portálu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c761-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2c761-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2c761-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2c761-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c761-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2c761-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c761-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2c761-133">Hello vyhledávacího pole zadejte **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="2c761-133">In hello search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="2c761-135">Na panelu výsledků hello vyberte **Amazon Web Services (AWS)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c761-135">In hello results panel, select **Amazon Web Services (AWS)**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c761-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c761-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c761-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2c761-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2c761-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Amazon Web Services (AWS) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c761-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Amazon Web Services (AWS) is tooa user in Azure AD.</span></span> <span data-ttu-id="2c761-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Amazon Web Services (AWS) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2c761-140">In other words, a link relationship between an Azure AD user and hello related user in Amazon Web Services (AWS) needs toobe established.</span></span>

<span data-ttu-id="2c761-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="2c761-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="2c761-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="2c761-142">tooconfigure and test Azure AD single sign-on with Amazon Web Services (AWS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2c761-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2c761-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2c761-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c761-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c761-145">**[Vytváření testovacího uživatele Amazon Web Services](#creating-an-amazon-web-services-test-user)**  -toohave protějšek Britta Simon v Amazon Web Services (AWS), která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c761-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - toohave a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="2c761-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c761-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c761-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2c761-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c761-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c761-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c761-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="2c761-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="2c761-150">**tooconfigure Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c761-150">**tooconfigure Azure AD single sign-on with Amazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="2c761-151">V portálu Azure, hello na hello **Amazon Web Services (AWS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2c761-151">In hello Azure Portal, on hello **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2c761-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c761-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="2c761-155">Na hello **Amazon Web Services (AWS) domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="2c761-155">On hello **Amazon Web Services (AWS) Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="2c761-157">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2c761-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="2c761-159">Hello Amazon Web Services (AWS) softwarová aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="2c761-159">hello Amazon Web Services (AWS) Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2c761-160">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c761-160">Please configure hello following claims for this application.</span></span> <span data-ttu-id="2c761-161">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c761-161">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2c761-162">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2c761-162">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="2c761-164">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c761-164">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="2c761-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2c761-165">Attribute Name</span></span>  | <span data-ttu-id="2c761-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2c761-166">Attribute Value</span></span> | <span data-ttu-id="2c761-167">obor názvů</span><span class="sxs-lookup"><span data-stu-id="2c761-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="2c761-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="2c761-168">RoleSessionName</span></span> | <span data-ttu-id="2c761-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="2c761-169">user.userprincipalname</span></span> | <span data-ttu-id="2c761-170">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="2c761-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="2c761-171">Role</span><span class="sxs-lookup"><span data-stu-id="2c761-171">Role</span></span>            | <span data-ttu-id="2c761-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="2c761-172">user.assignedroles</span></span> |  <span data-ttu-id="2c761-173">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="2c761-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="2c761-174">Je nutné zřizování v Azure AD toofetch všechny role hello z konzoly AWS tooconfigure hello uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2c761-174">You need tooconfigure hello user provisioning in Azure AD toofetch all hello roles from AWS Console.</span></span> <span data-ttu-id="2c761-175">Podrobnosti najdete níže uvedené kroky pro zřízení hello.</span><span class="sxs-lookup"><span data-stu-id="2c761-175">Please refer hello provisioning steps below.</span></span>

    <span data-ttu-id="2c761-176">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-176">a.</span></span> <span data-ttu-id="2c761-177">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c761-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="2c761-179">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-179">b.</span></span> <span data-ttu-id="2c761-180">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2c761-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="2c761-182">c.</span><span class="sxs-lookup"><span data-stu-id="2c761-182">c.</span></span> <span data-ttu-id="2c761-183">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2c761-183">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="2c761-184">Přidejte hodnotu Namespace hello jako ve výše uvedených.</span><span class="sxs-lookup"><span data-stu-id="2c761-184">Add hello Namespace value as given above.</span></span>
    
    <span data-ttu-id="2c761-185">d.</span><span class="sxs-lookup"><span data-stu-id="2c761-185">d.</span></span> <span data-ttu-id="2c761-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c761-186">Click **Ok**.</span></span>

7. <span data-ttu-id="2c761-187">Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení v Azure.</span><span class="sxs-lookup"><span data-stu-id="2c761-187">Click **Save** button toosave hello settings on Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2c761-189">V okně jiný prohlížeč, lokality společnosti Amazon Web Services (AWS) tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="2c761-189">In a different browser window, sign-on tooyour Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="2c761-190">Klikněte na tlačítko **konzoly domovské**.</span><span class="sxs-lookup"><span data-stu-id="2c761-190">Click **Console Home**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][11]

10. <span data-ttu-id="2c761-192">Klikněte na tlačítko **IAM** z **zabezpečení, Identity a dodržování předpisů** služby.</span><span class="sxs-lookup"><span data-stu-id="2c761-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Konfigurovat jednotné přihlašování][12]

11. <span data-ttu-id="2c761-194">Klikněte na tlačítko **zprostředkovatelů Identity**a potom klikněte na **vytvoření zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="2c761-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][13]

12. <span data-ttu-id="2c761-196">Na hello **konfigurace zprostředkovatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c761-196">On hello **Configure Provider** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování][14]
 
    <span data-ttu-id="2c761-198">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-198">a.</span></span> <span data-ttu-id="2c761-199">Jako **typ zprostředkovatele**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="2c761-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="2c761-200">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-200">b.</span></span> <span data-ttu-id="2c761-201">V hello **název zprostředkovatele** textovému poli, zadejte název zprostředkovatele (např: *službou WAAD*).</span><span class="sxs-lookup"><span data-stu-id="2c761-201">In hello **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="2c761-202">c.</span><span class="sxs-lookup"><span data-stu-id="2c761-202">c.</span></span> <span data-ttu-id="2c761-203">tooupload váš soubor stažený metadata, klikněte na tlačítko **zvolit soubor**.</span><span class="sxs-lookup"><span data-stu-id="2c761-203">tooupload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="2c761-204">d.</span><span class="sxs-lookup"><span data-stu-id="2c761-204">d.</span></span> <span data-ttu-id="2c761-205">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="2c761-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="2c761-206">Na hello **ověřte informace o poskytovateli** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2c761-206">On hello **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování][15]

14. <span data-ttu-id="2c761-208">Klikněte na tlačítko **role**a potom klikněte na **vytvořit novou roli**.</span><span class="sxs-lookup"><span data-stu-id="2c761-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování][16]

15. <span data-ttu-id="2c761-210">Na hello **nastavit název Role** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c761-210">On hello **Set Role Name** dialog, perform hello following steps:</span></span> 
    
    ![Konfigurovat jednotné přihlašování][17] 

    <span data-ttu-id="2c761-212">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-212">a.</span></span> <span data-ttu-id="2c761-213">V hello **název Role** textovému poli, zadejte název role (např: *TestUser*).</span><span class="sxs-lookup"><span data-stu-id="2c761-213">In hello **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="2c761-214">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-214">b.</span></span> <span data-ttu-id="2c761-215">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="2c761-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="2c761-216">Na hello **vyberte typ Role** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c761-216">On hello **Select Role Type** dialog, perform hello following steps:</span></span> 
    
    ![Konfigurovat jednotné přihlašování][18] 

    <span data-ttu-id="2c761-218">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-218">a.</span></span> <span data-ttu-id="2c761-219">Vyberte **Role pro přístup poskytovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="2c761-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="2c761-220">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-220">b.</span></span> <span data-ttu-id="2c761-221">V hello **Grant webové jednotné přihlašování (WebSSO) přístup tooSAML zprostředkovatelé** klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="2c761-221">In hello **Grant Web Single Sign-On (WebSSO) access tooSAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="2c761-222">Na hello **navázání vztahu důvěryhodnosti** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c761-222">On hello **Establish Trust** dialog, perform hello following steps:</span></span>  
    
    ![Konfigurovat jednotné přihlašování][19] 

    <span data-ttu-id="2c761-224">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-224">a.</span></span> <span data-ttu-id="2c761-225">Jako poskytovatel SAML, vyberte poskytovatele SAML hello jste dříve vytvořili (např: *službou WAAD*)</span><span class="sxs-lookup"><span data-stu-id="2c761-225">As SAML provider, select hello SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="2c761-226">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-226">b.</span></span> <span data-ttu-id="2c761-227">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="2c761-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="2c761-228">Na hello **ověřte důvěřovat Role** dialogové okno, klikněte na tlačítko **další krok**.</span><span class="sxs-lookup"><span data-stu-id="2c761-228">On hello **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Konfigurovat jednotné přihlašování][32]

19. <span data-ttu-id="2c761-230">Na hello **připojit zásady** dialogové okno, klikněte na tlačítko **další krok**.</span><span class="sxs-lookup"><span data-stu-id="2c761-230">On hello **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Konfigurovat jednotné přihlašování][33]

20. <span data-ttu-id="2c761-232">Na hello **zkontrolujte** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c761-232">On hello **Review** dialog, perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování][34]
 
    <span data-ttu-id="2c761-234">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-234">a.</span></span> <span data-ttu-id="2c761-235">Klikněte na tlačítko **vytvořit roli**.</span><span class="sxs-lookup"><span data-stu-id="2c761-235">Click **Create Role**.</span></span>

    <span data-ttu-id="2c761-236">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-236">b.</span></span> <span data-ttu-id="2c761-237">Vytvořit tolik role podle potřeby a jejich namapování toohello zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="2c761-237">Create as many roles as needed and map them toohello Identity Provider.</span></span>

21. <span data-ttu-id="2c761-238">Všechny role hello z AWS teď nakonfigurujte zřizování toofetch hello uživatelů</span><span class="sxs-lookup"><span data-stu-id="2c761-238">Now configure hello user provisioning toofetch all hello roles from AWS</span></span>

    <span data-ttu-id="2c761-239">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-239">a.</span></span> <span data-ttu-id="2c761-240">V přihlášení prostřednictvím konzoly AWS hello pomocí kořenového účtu</span><span class="sxs-lookup"><span data-stu-id="2c761-240">In hello AWS Console login with your root account</span></span>

    <span data-ttu-id="2c761-241">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-241">b.</span></span> <span data-ttu-id="2c761-242">Hello pravém horním rohu klikněte na své jméno a pak klikněte na hello **Moje zabezpečovací pověření** možnost.</span><span class="sxs-lookup"><span data-stu-id="2c761-242">In hello top right corner click your name and then click hello **My Security Credentials** option.</span></span> <span data-ttu-id="2c761-243">Tím se otevře na obrazovce jako upozornění.</span><span class="sxs-lookup"><span data-stu-id="2c761-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="2c761-244">Klikněte na tlačítko hello **zabezpečovací pověření** tlačítko toopass hello obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2c761-244">Click hello button **Security Credentials** button toopass hello screen.</span></span>
        
       ![Konfigurovat jednotné přihlašování][36]

       ![Konfigurovat jednotné přihlašování][37]

    <span data-ttu-id="2c761-247">c.</span><span class="sxs-lookup"><span data-stu-id="2c761-247">c.</span></span> <span data-ttu-id="2c761-248">V hello přístupové klíče oddílu, klikněte na tlačítko hello **vytvořit nový přístupový klíč** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c761-248">In hello Access Keys section click hello **Create New Access Key** button.</span></span> <span data-ttu-id="2c761-249">Tím se vytvoří hello ID přístupu klíč a hodnotu tokenu.</span><span class="sxs-lookup"><span data-stu-id="2c761-249">This generates hello Access Key ID and a token value.</span></span>
    
       ![Konfigurovat jednotné přihlašování][38]

    <span data-ttu-id="2c761-251">d.</span><span class="sxs-lookup"><span data-stu-id="2c761-251">d.</span></span> <span data-ttu-id="2c761-252">Zkopírujte oba tyto hodnoty a také, tak, aby neztratili ho stáhnout.</span><span class="sxs-lookup"><span data-stu-id="2c761-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="2c761-253">e.</span><span class="sxs-lookup"><span data-stu-id="2c761-253">e.</span></span> <span data-ttu-id="2c761-254">V hello Azure Portal, na stránce integrace aplikace hello Amazon Web Services (AWS), klikněte na **zřizování**.</span><span class="sxs-lookup"><span data-stu-id="2c761-254">In hello Azure Portal, on hello Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Konfigurovat jednotné přihlašování][35]

    <span data-ttu-id="2c761-256">f.</span><span class="sxs-lookup"><span data-stu-id="2c761-256">f.</span></span> <span data-ttu-id="2c761-257">Nastavit režim zřizování hello příliš**automatické**</span><span class="sxs-lookup"><span data-stu-id="2c761-257">Set hello Provisioning mode too**Automatic**</span></span>
        
       ![Konfigurovat jednotné přihlašování][39]

    <span data-ttu-id="2c761-259">g.</span><span class="sxs-lookup"><span data-stu-id="2c761-259">g.</span></span> <span data-ttu-id="2c761-260">Nyní ve hello **clientsecret** a **tajný klíč tokenu** vložit hello odpovídající hodnoty, které jste zkopírovali z konzoly AWS.</span><span class="sxs-lookup"><span data-stu-id="2c761-260">Now in hello **clientsecret** and **Secret Token** paste hello corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="2c761-261">h.</span><span class="sxs-lookup"><span data-stu-id="2c761-261">h.</span></span> <span data-ttu-id="2c761-262">Můžete kliknout na hello **Test připojení** tlačítko tootest hello připojení.</span><span class="sxs-lookup"><span data-stu-id="2c761-262">You can click hello **Test Connection** button tootest hello connectivity.</span></span> <span data-ttu-id="2c761-263">Po úspěšné, můžete začít hello zřizování konektor.</span><span class="sxs-lookup"><span data-stu-id="2c761-263">Once that is successful then you can start hello provisioning connector.</span></span>
       
       ![Konfigurovat jednotné přihlašování][40]

    <span data-ttu-id="2c761-265">i.</span><span class="sxs-lookup"><span data-stu-id="2c761-265">i.</span></span> <span data-ttu-id="2c761-266">Teď povolit hello Stav zřizování příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="2c761-266">Now enable hello Provisioning Status too**On**.</span></span> <span data-ttu-id="2c761-267">Tím se spustí načítání hello role z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2c761-267">This starts fetching hello roles from hello application.</span></span>

       ![Konfigurovat jednotné přihlašování][41]

    > [!NOTE]
    > <span data-ttu-id="2c761-269">Spuštění služby Azure AD zřizování každých po některé čas toosync hello role z AWS.</span><span class="sxs-lookup"><span data-stu-id="2c761-269">Azure AD Provisioning service runs every after some time toosync hello roles from AWS.</span></span> <span data-ttu-id="2c761-270">Měli byste vidět všechny hello zprostředkovatele Identity připojena AWS rolí do služby Azure AD a můžete je použít při přiřazování toousers aplikace hello nebo skupiny.</span><span class="sxs-lookup"><span data-stu-id="2c761-270">You should see all hello Identity Provider attached AWS roles into Azure AD and you can use them while assigning hello application toousers or groups.</span></span>

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c761-271">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c761-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c761-272">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2c761-272">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2c761-274">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c761-274">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c761-275">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c761-275">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2c761-277">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2c761-277">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c761-279">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c761-279">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c761-281">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c761-281">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2c761-283">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-283">a.</span></span> <span data-ttu-id="2c761-284">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c761-284">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c761-285">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-285">b.</span></span> <span data-ttu-id="2c761-286">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c761-286">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2c761-287">c.</span><span class="sxs-lookup"><span data-stu-id="2c761-287">c.</span></span> <span data-ttu-id="2c761-288">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2c761-288">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2c761-289">d.</span><span class="sxs-lookup"><span data-stu-id="2c761-289">d.</span></span> <span data-ttu-id="2c761-290">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2c761-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="2c761-291">Vytváření testovacího uživatele Amazon Web Services</span><span class="sxs-lookup"><span data-stu-id="2c761-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="2c761-292">V pořadí tooenable Azure AD Uživatelé toolog v tooAmazon Web Services (AWS) se musí zřízená do Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="2c761-292">In order tooenable Azure AD users toolog in tooAmazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="2c761-293">V případě hello Amazon Web Services (AWS) zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="2c761-293">In hello case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="2c761-294">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c761-294">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c761-295">Přihlaste se tooyour **Amazon Web Services (AWS)** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="2c761-295">Log in tooyour **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="2c761-296">Klikněte na tlačítko hello **konzoly domovské** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c761-296">Click hello **Console Home** icon.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][11]

3. <span data-ttu-id="2c761-298">Klikněte na tlačítko identita a správa přístupu.</span><span class="sxs-lookup"><span data-stu-id="2c761-298">Click Identity and Access Management.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][28]

4. <span data-ttu-id="2c761-300">V hello řídicí panel, klikněte na **uživatelé**a potom klikněte na **vytvořte nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2c761-300">In hello Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][29]

5. <span data-ttu-id="2c761-302">V dialogovém okně Vytvořit uživatele hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2c761-302">On hello Create User dialog, perform hello following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování][30]   
    
    <span data-ttu-id="2c761-304">a.</span><span class="sxs-lookup"><span data-stu-id="2c761-304">a.</span></span> <span data-ttu-id="2c761-305">V hello **zadejte uživatelská jména** textová pole, zadejte uživatelské jméno Brita Simon (userprincipalname) ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c761-305">In hello **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="2c761-306">b.</span><span class="sxs-lookup"><span data-stu-id="2c761-306">b.</span></span> <span data-ttu-id="2c761-307">Klikněte na tlačítko **vytvořit.**</span><span class="sxs-lookup"><span data-stu-id="2c761-307">Click **Create.**</span></span>
        
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2c761-308">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2c761-308">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2c761-309">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooAmazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="2c761-309">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAmazon Web Services (AWS).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2c761-311">**tooassign Britta Simon tooAmazon Web Services (AWS), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2c761-311">**tooassign Britta Simon tooAmazon Web Services (AWS), perform hello following steps:**</span></span>

1. <span data-ttu-id="2c761-312">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c761-312">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2c761-314">V seznamu aplikace hello vyberte **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="2c761-314">In hello applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="2c761-316">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2c761-316">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2c761-318">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c761-318">Click **Add** button.</span></span> <span data-ttu-id="2c761-319">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c761-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2c761-321">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2c761-321">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2c761-322">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c761-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c761-323">Na **vybrat roli** karty, vyberte hello vhodnou roli pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="2c761-323">On **Select Role** tab, select hello appropriate role for hello user.</span></span> <span data-ttu-id="2c761-324">Tyto role jsou zobrazeny s názvem role hello a název zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="2c761-324">All these roles are shown with hello role name and identity provider name.</span></span> <span data-ttu-id="2c761-325">Tímto způsobem můžete snadno identifikovat hello role z AWS.</span><span class="sxs-lookup"><span data-stu-id="2c761-325">This way you can easily identify hello roles from AWS.</span></span>

8. <span data-ttu-id="2c761-326">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c761-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2c761-327">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c761-327">Testing single sign-on</span></span>

<span data-ttu-id="2c761-328">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2c761-328">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2c761-329">Po kliknutí na tlačítko hello Amazon Web Services (AWS) dlaždici v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného Amazon Web Services (AWS) aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c761-329">When you click hello Amazon Web Services (AWS) tile in hello Access Panel, you should get automatically signed-on tooyour Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2c761-330">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2c761-330">Additional resources</span></span>

* [<span data-ttu-id="2c761-331">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c761-331">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c761-332">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c761-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
