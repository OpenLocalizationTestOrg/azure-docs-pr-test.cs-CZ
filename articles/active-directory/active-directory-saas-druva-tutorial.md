---
title: 'Kurz: Azure Active Directory integrace s Druva | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Druva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a><span data-ttu-id="2bab8-103">Kurz: Azure Active Directory integrace s Druva</span><span class="sxs-lookup"><span data-stu-id="2bab8-103">Tutorial: Azure Active Directory integration with Druva</span></span>

<span data-ttu-id="2bab8-104">V tomto kurzu zjistíte, jak toointegrate Druva s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2bab8-104">In this tutorial, you learn how toointegrate Druva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2bab8-105">Integrace Druva s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2bab8-105">Integrating Druva with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2bab8-106">Můžete ovládat ve službě Azure AD, který má přístup tooDruva.</span><span class="sxs-lookup"><span data-stu-id="2bab8-106">You can control in Azure AD who has access tooDruva.</span></span>
- <span data-ttu-id="2bab8-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDruva (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bab8-107">You can enable your users tooautomatically get signed-on tooDruva (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="2bab8-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2bab8-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="2bab8-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2bab8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bab8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2bab8-110">Prerequisites</span></span>

<span data-ttu-id="2bab8-111">Integrace služby Azure AD s Druva tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2bab8-111">tooconfigure Azure AD integration with Druva, you need hello following items:</span></span>

- <span data-ttu-id="2bab8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2bab8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2bab8-113">Druva jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2bab8-113">A Druva single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2bab8-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2bab8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2bab8-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2bab8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2bab8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2bab8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2bab8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2bab8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2bab8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2bab8-118">Scenario description</span></span>
<span data-ttu-id="2bab8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2bab8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2bab8-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2bab8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2bab8-121">Přidání Druva z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2bab8-121">Adding Druva from hello gallery</span></span>
2. <span data-ttu-id="2bab8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2bab8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-druva-from-hello-gallery"></a><span data-ttu-id="2bab8-123">Přidání Druva z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2bab8-123">Adding Druva from hello gallery</span></span>
<span data-ttu-id="2bab8-124">tooconfigure hello integrace Druva do Azure AD, je nutné tooadd Druva hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2bab8-124">tooconfigure hello integration of Druva into Azure AD, you need tooadd Druva from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2bab8-125">**tooadd Druva z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2bab8-125">**tooadd Druva from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bab8-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2bab8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![tlačítko Azure Active Directory Hello][1]

2. <span data-ttu-id="2bab8-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2bab8-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-129">Then go too**All applications**.</span></span>

    ![okno aplikace Hello Enterprise][2]
    
3. <span data-ttu-id="2bab8-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2bab8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![tlačítko nové aplikace Hello][3]

4. <span data-ttu-id="2bab8-133">Hello vyhledávacího pole zadejte **Druva**, vyberte **Druva** z panelu výsledků klikněte **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bab8-133">In hello search box, type **Druva**, select **Druva** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Druva v seznamu výsledků hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2bab8-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2bab8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="2bab8-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Druva podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2bab8-136">In this section, you configure and test Azure AD single sign-on with Druva based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2bab8-137">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Druva je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bab8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Druva is tooa user in Azure AD.</span></span> <span data-ttu-id="2bab8-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Druva musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2bab8-138">In other words, a link relationship between an Azure AD user and hello related user in Druva needs toobe established.</span></span>

<span data-ttu-id="2bab8-139">V Druva, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2bab8-139">In Druva, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2bab8-140">tooconfigure a testu Azure AD jednotné přihlašování s Druva, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="2bab8-140">tooconfigure and test Azure AD single sign-on with Druva, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2bab8-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2bab8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2bab8-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2bab8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2bab8-143">**[Vytvoření zkušebního uživatele Druva](#create-a-druva-test-user)**  -toohave protějšek Britta Simon v Druva, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2bab8-143">**[Create a Druva test user](#create-a-druva-test-user)** - toohave a counterpart of Britta Simon in Druva that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2bab8-144">**[Přiřadit hello Azure AD testovacího uživatele](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2bab8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2bab8-145">**[Test jednotného přihlašování](#test-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2bab8-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="2bab8-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2bab8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="2bab8-147">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Druva.</span><span class="sxs-lookup"><span data-stu-id="2bab8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Druva application.</span></span>

<span data-ttu-id="2bab8-148">**tooconfigure Azure AD jednotné přihlašování s Druva, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2bab8-148">**tooconfigure Azure AD single sign-on with Druva, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bab8-149">V portálu Azure, na hello hello **Druva** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-149">In hello Azure portal, on hello **Druva** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="2bab8-151">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2bab8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. <span data-ttu-id="2bab8-153">Na hello **Druva domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2bab8-153">On hello **Druva Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    <span data-ttu-id="2bab8-155">V hello **přihlašovací adresa URL** textovému poli, hello zadat adresu URL:`https://cloud.druva.com/home`</span><span class="sxs-lookup"><span data-stu-id="2bab8-155">In hello **Sign-on URL** textbox, type hello URL: `https://cloud.druva.com/home`</span></span>

4. <span data-ttu-id="2bab8-156">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2bab8-156">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![odkaz ke stažení certifikátu Hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. <span data-ttu-id="2bab8-158">Aplikace Druva očekává hello SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste tooadd vlastních atributů mapování tooyour **atributy tokenu SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2bab8-158">Your Druva application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. <span data-ttu-id="2bab8-160">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello předcházející bitové kopie a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2bab8-160">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="2bab8-161">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2bab8-161">Attribute Name</span></span>      | <span data-ttu-id="2bab8-162">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2bab8-162">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="2bab8-163">synchronizována\_auth\_tokenu</span><span class="sxs-lookup"><span data-stu-id="2bab8-163">insync\_auth\_token</span></span> |<span data-ttu-id="2bab8-164">Zadejte hodnotu tokenu generovaného hello</span><span class="sxs-lookup"><span data-stu-id="2bab8-164">Enter hello token generated value</span></span> |
    
    <span data-ttu-id="2bab8-165">a.</span><span class="sxs-lookup"><span data-stu-id="2bab8-165">a.</span></span> <span data-ttu-id="2bab8-166">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-166">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="2bab8-169">b.</span><span class="sxs-lookup"><span data-stu-id="2bab8-169">b.</span></span> <span data-ttu-id="2bab8-170">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2bab8-170">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="2bab8-171">c.</span><span class="sxs-lookup"><span data-stu-id="2bab8-171">c.</span></span> <span data-ttu-id="2bab8-172">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2bab8-172">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="2bab8-173">Hodnota tokenu generovaného Hello se vysvětluje dále v kurzu.</span><span class="sxs-lookup"><span data-stu-id="2bab8-173">hello token generated value is explained later in tutorial.</span></span>
    
    <span data-ttu-id="2bab8-174">d.</span><span class="sxs-lookup"><span data-stu-id="2bab8-174">d.</span></span> <span data-ttu-id="2bab8-175">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-175">Click **Ok**.</span></span>    

7. <span data-ttu-id="2bab8-176">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2bab8-176">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2bab8-178">Na hello **Druva konfigurace** klikněte na tlačítko **konfigurace Druva** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-178">On hello **Druva Configuration** section, click **Configure Druva** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2bab8-179">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2bab8-179">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. <span data-ttu-id="2bab8-181">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti Druva tooyour.</span><span class="sxs-lookup"><span data-stu-id="2bab8-181">In a different web browser window, log in tooyour Druva company site as an administrator.</span></span>

10. <span data-ttu-id="2bab8-182">Přejděte příliš**spravovat \> nastavení**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-182">Go too**Manage \> Settings**.</span></span>

    <span data-ttu-id="2bab8-183">![Nastavení](./media/active-directory-saas-druva-tutorial/ic795091.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="2bab8-183">![Settings](./media/active-directory-saas-druva-tutorial/ic795091.png "Settings")</span></span>

11. <span data-ttu-id="2bab8-184">V dialogovém okně Nastavení jednotného přihlašování hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2bab8-184">On hello Single Sign-On Settings dialog, perform hello following steps:</span></span>

    <span data-ttu-id="2bab8-185">![Jednotné přihlašování v nastavení](./media/active-directory-saas-druva-tutorial/ic795092.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="2bab8-185">![Single Sign-On Settings](./media/active-directory-saas-druva-tutorial/ic795092.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="2bab8-186">a.</span><span class="sxs-lookup"><span data-stu-id="2bab8-186">a.</span></span> <span data-ttu-id="2bab8-187">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **ID zprostředkovatele přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2bab8-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **ID Provider Login URL** textbox.</span></span>
    
    <span data-ttu-id="2bab8-188">b.</span><span class="sxs-lookup"><span data-stu-id="2bab8-188">b.</span></span> <span data-ttu-id="2bab8-189">Vložení **Sign-Out URL** hodnotu, kterou jste zkopírovali z hello portálu Azure do hello **adresy URL odhlašovací ID zprostředkovatele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2bab8-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **ID Provider Logout URL** textbox.</span></span>
    
     <span data-ttu-id="2bab8-190">c.</span><span class="sxs-lookup"><span data-stu-id="2bab8-190">c.</span></span> <span data-ttu-id="2bab8-191">Otevřete váš kódování base-64 kódovaného certifikátu v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **certifikát poskytovatele ID** textbox</span><span class="sxs-lookup"><span data-stu-id="2bab8-191">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **ID Provider Certificate** textbox</span></span>
     
     <span data-ttu-id="2bab8-192">d.</span><span class="sxs-lookup"><span data-stu-id="2bab8-192">d.</span></span> <span data-ttu-id="2bab8-193">tooopen hello **nastavení** klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-193">tooopen hello **Settings** page, click **Save**.</span></span>

12. <span data-ttu-id="2bab8-194">Na hello **nastavení** klikněte na tlačítko **vygenerovat Token jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-194">On hello **Settings** page, click **Generate SSO Token**.</span></span>

    <span data-ttu-id="2bab8-195">![Nastavení](./media/active-directory-saas-druva-tutorial/ic795093.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="2bab8-195">![Settings](./media/active-directory-saas-druva-tutorial/ic795093.png "Settings")</span></span>

13. <span data-ttu-id="2bab8-196">Na hello **jeden přihlašování ověřování tokenu** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2bab8-196">On hello **Single Sign-on Authentication Token** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="2bab8-197">![Token jednotného přihlašování k](./media/active-directory-saas-druva-tutorial/ic795094.png "tokenu jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="2bab8-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO Token")</span></span>
    
    <span data-ttu-id="2bab8-198">a.</span><span class="sxs-lookup"><span data-stu-id="2bab8-198">a.</span></span> <span data-ttu-id="2bab8-199">Klikněte na tlačítko **kopie**, vložte zkopírovaný hodnotu v hello **hodnotu** textového pole v hello **přidat atribut** části.</span><span class="sxs-lookup"><span data-stu-id="2bab8-199">Click **Copy**, Paste copied value in hello **Value** textbox in hello **Add Attribute** section.</span></span>
    
    <span data-ttu-id="2bab8-200">b.</span><span class="sxs-lookup"><span data-stu-id="2bab8-200">b.</span></span> <span data-ttu-id="2bab8-201">Klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-201">Click **Close**.</span></span>

> [!TIP]
> <span data-ttu-id="2bab8-202">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2bab8-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2bab8-203">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2bab8-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2bab8-204">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2bab8-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2bab8-205">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2bab8-205">Create an Azure AD test user</span></span>

<span data-ttu-id="2bab8-206">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2bab8-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="2bab8-208">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2bab8-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bab8-209">V hello portál Azure, v levém podokně hello, klikněte na tlačítko hello **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2bab8-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![tlačítko Azure Active Directory Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="2bab8-211">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hello "Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="2bab8-213">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello horní části hello **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![tlačítko Přidat Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="2bab8-215">V hello **uživatele** dialogové okno pole, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2bab8-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![Dialogové okno uživatelského Hello](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    <span data-ttu-id="2bab8-217">a.</span><span class="sxs-lookup"><span data-stu-id="2bab8-217">a.</span></span> <span data-ttu-id="2bab8-218">V hello **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2bab8-219">b.</span><span class="sxs-lookup"><span data-stu-id="2bab8-219">b.</span></span> <span data-ttu-id="2bab8-220">V hello **uživatelské jméno** pole typu hello e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2bab8-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="2bab8-221">c.</span><span class="sxs-lookup"><span data-stu-id="2bab8-221">c.</span></span> <span data-ttu-id="2bab8-222">Vyberte hello **zobrazit hesla** zaškrtněte políčko a zapište si ji hello hodnotu, která se zobrazí v hello **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="2bab8-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="2bab8-223">d.</span><span class="sxs-lookup"><span data-stu-id="2bab8-223">d.</span></span> <span data-ttu-id="2bab8-224">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-224">Click **Create**.</span></span>
 
### <a name="create-a-druva-test-user"></a><span data-ttu-id="2bab8-225">Vytvoření zkušebního uživatele Druva</span><span class="sxs-lookup"><span data-stu-id="2bab8-225">Create a Druva test user</span></span>

<span data-ttu-id="2bab8-226">V pořadí tooenable Azure AD Uživatelé toolog v tooDruva musí být zřízená do Druva.</span><span class="sxs-lookup"><span data-stu-id="2bab8-226">In order tooenable Azure AD users toolog in tooDruva, they must be provisioned into Druva.</span></span> <span data-ttu-id="2bab8-227">V případě hello Druva zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="2bab8-227">In hello case of Druva, provisioning is a manual task.</span></span>

<span data-ttu-id="2bab8-228">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2bab8-228">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bab8-229">Přihlaste se tooyour **Druva** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="2bab8-229">Log in tooyour **Druva** company site as administrator.</span></span>

2. <span data-ttu-id="2bab8-230">Přejděte příliš**spravovat \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-230">Go too**Manage \> Users**.</span></span>
   
   <span data-ttu-id="2bab8-231">![Správa uživatelů](./media/active-directory-saas-druva-tutorial/ic795097.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="2bab8-231">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795097.png "Manage Users")</span></span>

3. <span data-ttu-id="2bab8-232">Klikněte na tlačítko **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-232">Click **Create New**.</span></span>
   
   <span data-ttu-id="2bab8-233">![Správa uživatelů](./media/active-directory-saas-druva-tutorial/ic795098.png "Správa uživatelů")</span><span class="sxs-lookup"><span data-stu-id="2bab8-233">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795098.png "Manage Users")</span></span>

4. <span data-ttu-id="2bab8-234">V dialogovém okně Vytvořit nového uživatele hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2bab8-234">On hello Create New User dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="2bab8-235">![Vytvoření NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "vytvořit NewUser")</span><span class="sxs-lookup"><span data-stu-id="2bab8-235">![Create NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "Create NewUser")</span></span>
   
   <span data-ttu-id="2bab8-236">a.</span><span class="sxs-lookup"><span data-stu-id="2bab8-236">a.</span></span> <span data-ttu-id="2bab8-237">V hello **e-mailová adresa** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2bab8-237">In hello **Email address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="2bab8-238">b.</span><span class="sxs-lookup"><span data-stu-id="2bab8-238">b.</span></span> <span data-ttu-id="2bab8-239">V hello **název** textovému poli, zadejte název hello uživatele jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-239">In hello **Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="2bab8-240">c.</span><span class="sxs-lookup"><span data-stu-id="2bab8-240">c.</span></span> <span data-ttu-id="2bab8-241">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-241">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="2bab8-242">Můžete použít všechny ostatní Druva uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Druva tooprovision uživatelské účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bab8-242">You can use any other Druva user account creation tools or APIs provided by Druva tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="2bab8-243">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2bab8-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="2bab8-244">V této části povolíte tak, že udělíte přístup tooDruva toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2bab8-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDruva.</span></span>

![Přiřadit role uživatele hello][200] 

<span data-ttu-id="2bab8-246">**tooassign Britta Simon tooDruva, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2bab8-246">**tooassign Britta Simon tooDruva, perform hello following steps:**</span></span>

1. <span data-ttu-id="2bab8-247">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2bab8-249">V seznamu aplikace hello vyberte **Druva**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-249">In hello applications list, select **Druva**.</span></span>

    ![v seznamu aplikace hello Hello Druva odkaz](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. <span data-ttu-id="2bab8-251">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2bab8-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![odkaz "Uživatelé a skupiny" Hello][202]

4. <span data-ttu-id="2bab8-253">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2bab8-253">Click **Add** button.</span></span> <span data-ttu-id="2bab8-254">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Podokno Přidat přidružení Hello][203]

5. <span data-ttu-id="2bab8-256">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2bab8-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2bab8-257">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2bab8-258">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2bab8-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2bab8-259">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2bab8-259">Test single sign-on</span></span>

<span data-ttu-id="2bab8-260">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2bab8-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2bab8-261">Když kliknete na dlaždici Druva hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Druva aplikace.</span><span class="sxs-lookup"><span data-stu-id="2bab8-261">When you click hello Druva tile in hello Access Panel, you should get automatically signed-on tooyour Druva application.</span></span>
<span data-ttu-id="2bab8-262">Další informace o na přístupovém panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2bab8-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2bab8-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2bab8-263">Additional resources</span></span>

* [<span data-ttu-id="2bab8-264">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2bab8-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2bab8-265">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2bab8-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

