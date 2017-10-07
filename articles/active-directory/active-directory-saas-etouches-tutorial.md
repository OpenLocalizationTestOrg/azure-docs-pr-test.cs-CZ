---
title: 'Kurz: Azure Active Directory integrace s etouches | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a etouches."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5f3ff7550e660b0fc52612140ca55061504b5edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="5f2b0-103">Kurz: Azure Active Directory integrace s etouches</span><span class="sxs-lookup"><span data-stu-id="5f2b0-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="5f2b0-104">V tomto kurzu zjistíte, jak toointegrate etouches s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5f2b0-104">In this tutorial, you learn how toointegrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5f2b0-105">Integrace etouches s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-105">Integrating etouches with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5f2b0-106">Můžete řídit ve službě Azure AD, který má přístup tooetouches</span><span class="sxs-lookup"><span data-stu-id="5f2b0-106">You can control in Azure AD who has access tooetouches</span></span>
- <span data-ttu-id="5f2b0-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooetouches (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f2b0-107">You can enable your users tooautomatically get signed-on tooetouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5f2b0-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f2b0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5f2b0-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5f2b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f2b0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f2b0-110">Prerequisites</span></span>

<span data-ttu-id="5f2b0-111">Integrace služby Azure AD s etouches tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-111">tooconfigure Azure AD integration with etouches, you need hello following items:</span></span>

- <span data-ttu-id="5f2b0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f2b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5f2b0-113">Etouches jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5f2b0-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5f2b0-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5f2b0-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5f2b0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5f2b0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f2b0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5f2b0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5f2b0-118">Scenario description</span></span>
<span data-ttu-id="5f2b0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5f2b0-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5f2b0-121">Přidání etouches z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f2b0-121">Adding etouches from hello gallery</span></span>
2. <span data-ttu-id="5f2b0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f2b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-hello-gallery"></a><span data-ttu-id="5f2b0-123">Přidání etouches z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5f2b0-123">Adding etouches from hello gallery</span></span>
<span data-ttu-id="5f2b0-124">tooconfigure hello integrace etouches do Azure AD, je nutné tooadd etouches hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-124">tooconfigure hello integration of etouches into Azure AD, you need tooadd etouches from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5f2b0-125">**tooadd etouches z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f2b0-125">**tooadd etouches from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f2b0-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="5f2b0-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5f2b0-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="5f2b0-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="5f2b0-133">Hello vyhledávacího pole zadejte **etouches**, vyberte **etouches** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-133">In hello search box, type **etouches**, select **etouches** from result panel then click **Add** button tooadd hello application.</span></span>

    ![etouches v seznamu výsledků hello](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5f2b0-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f2b0-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="5f2b0-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s etouches podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="5f2b0-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5f2b0-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v etouches je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in etouches is tooa user in Azure AD.</span></span> <span data-ttu-id="5f2b0-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v etouches musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-138">In other words, a link relationship between an Azure AD user and hello related user in etouches needs toobe established.</span></span>

<span data-ttu-id="5f2b0-139">V etouches, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-139">In etouches, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5f2b0-140">tooconfigure a testu Azure AD jednotné přihlašování s etouches, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-140">tooconfigure and test Azure AD single sign-on with etouches, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5f2b0-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5f2b0-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5f2b0-143">**[Vytvořit testovací uživatele s etouches](#create-an-etouches-test-user)**  -toohave protějšek Britta Simon v etouches, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-143">**[Create an etouches test user](#create-an-etouches-test-user)** - toohave a counterpart of Britta Simon in etouches that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5f2b0-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5f2b0-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5f2b0-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f2b0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5f2b0-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci etouches.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="5f2b0-148">**tooconfigure Azure AD jednotné přihlašování s etouches, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f2b0-148">**tooconfigure Azure AD single sign-on with etouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f2b0-149">V portálu Azure, na hello hello **etouches** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-149">In hello Azure portal, on hello **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5f2b0-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="5f2b0-153">Na hello **etouches domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-153">On hello **etouches Domain and URLs** section, perform hello following steps:</span></span>

    ![jednotné přihlašování informace etouches domény a adresy URL](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="5f2b0-155">a.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-155">a.</span></span> <span data-ttu-id="5f2b0-156">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="5f2b0-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="5f2b0-157">b.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-157">b.</span></span> <span data-ttu-id="5f2b0-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="5f2b0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5f2b0-159">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-159">These values are not real.</span></span> <span data-ttu-id="5f2b0-160">Aktualizujte hodnotu hello s hello skutečné přihlášení adresy URL a identifikátor, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-160">You update hello value with hello actual Sign on URL and Identifier, which is explained later in hello tutorial.</span></span>
    > 

4. <span data-ttu-id="5f2b0-161">aplikace etouches očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-161">etouches application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="5f2b0-162">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-162">Configure hello following claims for this application.</span></span> <span data-ttu-id="5f2b0-163">Můžete spravovat hello hodnoty těchto atributů z hello **atribut uživatele** aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-163">You can manage hello values of these attributes from hello **User Attribute** of hello application.</span></span> <span data-ttu-id="5f2b0-164">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-164">hello following screenshot shows an example for this.</span></span> 

    ![Atribut uživatele](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="5f2b0-166">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-166">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="5f2b0-167">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5f2b0-167">Attribute Name</span></span> | <span data-ttu-id="5f2b0-168">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5f2b0-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="5f2b0-169">E-mail</span><span class="sxs-lookup"><span data-stu-id="5f2b0-169">Email</span></span> | <span data-ttu-id="5f2b0-170">User.Mail</span><span class="sxs-lookup"><span data-stu-id="5f2b0-170">user.mail</span></span> |    
    
    <span data-ttu-id="5f2b0-171">a.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-171">a.</span></span> <span data-ttu-id="5f2b0-172">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-172">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Přidání atributu](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Atribut dialogové okno Přidání](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="5f2b0-175">b.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-175">b.</span></span> <span data-ttu-id="5f2b0-176">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-176">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="5f2b0-177">c.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-177">c.</span></span> <span data-ttu-id="5f2b0-178">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-178">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="5f2b0-179">d.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-179">d.</span></span> <span data-ttu-id="5f2b0-180">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="5f2b0-181">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-181">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="5f2b0-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-183">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5f2b0-185">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, proveďte následující kroky v aplikaci etouches hello hello:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-185">tooget SSO configured for your application, perform hello following steps in hello etouches application:</span></span> 

    ![Konfigurace etouches](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="5f2b0-187">a.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-187">a.</span></span> <span data-ttu-id="5f2b0-188">Přihlášení příliš**etouches** aplikace pomocí hello práva správce.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-188">Login too**etouches** application using hello Admin rights.</span></span>
   
    <span data-ttu-id="5f2b0-189">b.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-189">b.</span></span> <span data-ttu-id="5f2b0-190">Přejděte toohello **SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-190">Go toohello **SAML** Configuration.</span></span>

    <span data-ttu-id="5f2b0-191">c.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-191">c.</span></span> <span data-ttu-id="5f2b0-192">V hello **obecné nastavení** část, otevřete svůj certifikát stažený z portálu Azure v poznámkovém bloku hello kopírování obsahu a pak ji vložit do textového pole hello IDP metadata.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-192">In hello **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy hello content, and then paste it into hello IDP metadata textbox.</span></span> 

    <span data-ttu-id="5f2b0-193">d.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-193">d.</span></span> <span data-ttu-id="5f2b0-194">Klikněte na hello **Uložit & zůstat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-194">Click on hello **Save & Stay** button.</span></span>
  
    <span data-ttu-id="5f2b0-195">e.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-195">e.</span></span> <span data-ttu-id="5f2b0-196">Klikněte na hello **Metadata aktualizace** tlačítka na hello oddílem metadat SAML.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-196">Click on hello **Update Metadata** button in hello SAML Metadata section.</span></span> 

    <span data-ttu-id="5f2b0-197">f.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-197">f.</span></span> <span data-ttu-id="5f2b0-198">Tato otevře hello stránky a provádět jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-198">This opens hello page and perform SSO.</span></span> <span data-ttu-id="5f2b0-199">Jednou hello jednotné přihlašování funguje pak můžete nastavit hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-199">Once hello SSO is working then you can set up hello username.</span></span>

    <span data-ttu-id="5f2b0-200">g.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-200">g.</span></span> <span data-ttu-id="5f2b0-201">V poli hello uživatelské jméno, vyberte hello **emailaddress** jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-201">In hello Username field, select hello **emailaddress** as shown in hello image below.</span></span> 

    <span data-ttu-id="5f2b0-202">h.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-202">h.</span></span> <span data-ttu-id="5f2b0-203">Kopírování hello **SP entity ID** a vložte ji do hello **identifikátor** textovému poli, která je v **etouches domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-203">Copy hello **SP entity ID** value and paste it into hello **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="5f2b0-204">i.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-204">i.</span></span> <span data-ttu-id="5f2b0-205">Kopírování hello **URL jednotného přihlašování nebo ACS** a vložte ji do hello **přihlásit na adrese URL** textovému poli, která je v **etouches domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-205">Copy hello **SSO URL / ACS** value and paste it into hello **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="5f2b0-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5f2b0-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5f2b0-207">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5f2b0-208">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5f2b0-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5f2b0-209">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f2b0-209">Create an Azure AD test user</span></span>
<span data-ttu-id="5f2b0-210">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="5f2b0-212">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f2b0-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f2b0-213">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5f2b0-215">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5f2b0-217">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![tlačítko Přidat Hello](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5f2b0-219">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f2b0-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5f2b0-221">a.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-221">a.</span></span> <span data-ttu-id="5f2b0-222">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5f2b0-223">b.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-223">b.</span></span> <span data-ttu-id="5f2b0-224">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5f2b0-225">c.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-225">c.</span></span> <span data-ttu-id="5f2b0-226">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5f2b0-227">d.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-227">d.</span></span> <span data-ttu-id="5f2b0-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="5f2b0-229">Vytvořit uživatele s etouches testu</span><span class="sxs-lookup"><span data-stu-id="5f2b0-229">Create an etouches test user</span></span>

<span data-ttu-id="5f2b0-230">V této části vytvoříte volal Britta Simon v etouches uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="5f2b0-231">Práce s [tým podpory etouches klienta](https://www.etouches.com/event-software/support/customer-support/) tooadd hello uživatelé v platformě etouches hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) tooadd hello users in hello etouches platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5f2b0-232">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5f2b0-232">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5f2b0-233">V této části povolíte tak, že udělíte přístup tooetouches toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooetouches.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="5f2b0-235">**tooassign Britta Simon tooetouches, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5f2b0-235">**tooassign Britta Simon tooetouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="5f2b0-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5f2b0-238">V seznamu aplikace hello vyberte **etouches**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-238">In hello applications list, select **etouches**.</span></span>

    ![v seznamu aplikace hello Hello etouches odkaz](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="5f2b0-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202] 

4. <span data-ttu-id="5f2b0-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-242">Click **Add** button.</span></span> <span data-ttu-id="5f2b0-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="5f2b0-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5f2b0-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5f2b0-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5f2b0-248">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f2b0-248">Test single sign-on</span></span>


<span data-ttu-id="5f2b0-249">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-249">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5f2b0-250">Po kliknutí na tlačítko hello etouches dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour etouches aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f2b0-250">When you click hello etouches tile in hello Access Panel, you should get automatically signed-on tooyour etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f2b0-251">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5f2b0-251">Additional resources</span></span>

* [<span data-ttu-id="5f2b0-252">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f2b0-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5f2b0-253">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5f2b0-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

