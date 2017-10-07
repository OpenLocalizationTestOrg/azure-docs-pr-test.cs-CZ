---
title: 'Kurz: Azure Active Directory integrace s Domo | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Domo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: cc70f8e5013f864d275762bbc1f84bd9677e8c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="976f5-103">Kurz: Azure Active Directory integrace s Domo</span><span class="sxs-lookup"><span data-stu-id="976f5-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="976f5-104">V tomto kurzu zjistíte, jak toointegrate Domo s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="976f5-104">In this tutorial, you learn how toointegrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="976f5-105">Integrace Domo s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="976f5-105">Integrating Domo with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="976f5-106">Můžete řídit ve službě Azure AD, který má přístup tooDomo</span><span class="sxs-lookup"><span data-stu-id="976f5-106">You can control in Azure AD who has access tooDomo</span></span>
- <span data-ttu-id="976f5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDomo (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="976f5-107">You can enable your users tooautomatically get signed-on tooDomo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="976f5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="976f5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="976f5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="976f5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="976f5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="976f5-110">Prerequisites</span></span>

<span data-ttu-id="976f5-111">Integrace služby Azure AD s Domo tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="976f5-111">tooconfigure Azure AD integration with Domo, you need hello following items:</span></span>

- <span data-ttu-id="976f5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="976f5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="976f5-113">Domo jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="976f5-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="976f5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="976f5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="976f5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="976f5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="976f5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="976f5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="976f5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="976f5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="976f5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="976f5-118">Scenario description</span></span>
<span data-ttu-id="976f5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="976f5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="976f5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="976f5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="976f5-121">Přidání Domo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="976f5-121">Adding Domo from hello gallery</span></span>
2. <span data-ttu-id="976f5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="976f5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-hello-gallery"></a><span data-ttu-id="976f5-123">Přidání Domo z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="976f5-123">Adding Domo from hello gallery</span></span>
<span data-ttu-id="976f5-124">tooconfigure hello integrace Domo do Azure AD, je nutné tooadd Domo hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="976f5-124">tooconfigure hello integration of Domo into Azure AD, you need tooadd Domo from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="976f5-125">**tooadd Domo z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="976f5-125">**tooadd Domo from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="976f5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="976f5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="976f5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="976f5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="976f5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="976f5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="976f5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="976f5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="976f5-133">Hello vyhledávacího pole zadejte **Domo**.</span><span class="sxs-lookup"><span data-stu-id="976f5-133">In hello search box, type **Domo**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="976f5-135">Na panelu výsledků hello vyberte **Domo**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="976f5-135">In hello results panel, select **Domo**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="976f5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="976f5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="976f5-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Domo podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="976f5-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="976f5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Domo je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="976f5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Domo is tooa user in Azure AD.</span></span> <span data-ttu-id="976f5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Domo musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="976f5-140">In other words, a link relationship between an Azure AD user and hello related user in Domo needs toobe established.</span></span>

<span data-ttu-id="976f5-141">V Domo, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="976f5-141">In Domo, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="976f5-142">tooconfigure a testu Azure AD jednotné přihlašování s Domo, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="976f5-142">tooconfigure and test Azure AD single sign-on with Domo, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="976f5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="976f5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="976f5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="976f5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="976f5-145">**[Vytvoření zkušebního uživatele Domo](#creating-a-domo-test-user)**  -toohave protějšek Britta Simon v Domo, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="976f5-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - toohave a counterpart of Britta Simon in Domo that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="976f5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="976f5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="976f5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="976f5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="976f5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="976f5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="976f5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Domo.</span><span class="sxs-lookup"><span data-stu-id="976f5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="976f5-150">**tooconfigure Azure AD jednotné přihlašování s Domo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="976f5-150">**tooconfigure Azure AD single sign-on with Domo, perform hello following steps:**</span></span>

1. <span data-ttu-id="976f5-151">V portálu Azure, na hello hello **Domo** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="976f5-151">In hello Azure portal, on hello **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="976f5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="976f5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="976f5-155">Na hello **Domo domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="976f5-155">On hello **Domo Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="976f5-157">a.</span><span class="sxs-lookup"><span data-stu-id="976f5-157">a.</span></span> <span data-ttu-id="976f5-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="976f5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="976f5-159">b.</span><span class="sxs-lookup"><span data-stu-id="976f5-159">b.</span></span> <span data-ttu-id="976f5-160">V hello **identifikátor** textové pole, zadejte adresu URL pomocí hello následující vzory:</span><span class="sxs-lookup"><span data-stu-id="976f5-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="976f5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="976f5-161">These values are not real.</span></span> <span data-ttu-id="976f5-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="976f5-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="976f5-163">Obraťte se na [tým podpory Domo klienta](mailto:support@domo.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="976f5-163">Contact [Domo Client support team](mailto:support@domo.com) tooget these values.</span></span>

4. <span data-ttu-id="976f5-164">Aplikace Domo očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="976f5-164">Domo application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="976f5-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="976f5-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="976f5-166">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="976f5-166">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="976f5-167">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="976f5-167">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="976f5-169">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="976f5-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="976f5-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="976f5-170">Attribute Name</span></span> | <span data-ttu-id="976f5-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="976f5-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="976f5-172">jméno</span><span class="sxs-lookup"><span data-stu-id="976f5-172">name</span></span> | <span data-ttu-id="976f5-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="976f5-173">user.displayname</span></span> |
    | <span data-ttu-id="976f5-174">E-mailu</span><span class="sxs-lookup"><span data-stu-id="976f5-174">email</span></span> | <span data-ttu-id="976f5-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="976f5-175">user.mail</span></span> |
    
    <span data-ttu-id="976f5-176">a.</span><span class="sxs-lookup"><span data-stu-id="976f5-176">a.</span></span> <span data-ttu-id="976f5-177">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="976f5-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="976f5-180">b.</span><span class="sxs-lookup"><span data-stu-id="976f5-180">b.</span></span> <span data-ttu-id="976f5-181">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="976f5-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="976f5-182">c.</span><span class="sxs-lookup"><span data-stu-id="976f5-182">c.</span></span> <span data-ttu-id="976f5-183">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="976f5-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="976f5-184">d.</span><span class="sxs-lookup"><span data-stu-id="976f5-184">d.</span></span> <span data-ttu-id="976f5-185">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="976f5-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="976f5-186">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="976f5-186">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="976f5-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="976f5-188">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="976f5-190">Na hello **Domo konfigurace** klikněte na tlačítko **konfigurace Domo** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="976f5-190">On hello **Domo Configuration** section, click **Configure Domo** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="976f5-191">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="976f5-191">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span> 

   ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="976f5-193">tooconfigure jednotného přihlašování na **Domo** straně, je nutné stáhnout hello toosend **certifikát**, **SAML Entity ID**, hello **SAML-služby přihlášení Adresa URL** a hello **Sign-Out URL** příliš[tým podpory Domo](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="976f5-193">tooconfigure single sign-on on **Domo** side, you need toosend hello downloaded **Certificate**, **SAML Entity ID**, hello **SAML Single Sign-On Service URL** and hello **Sign-Out URL** too[Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="976f5-194">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="976f5-194">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="976f5-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="976f5-195">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="976f5-196">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="976f5-196">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="976f5-197">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="976f5-197">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="976f5-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="976f5-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="976f5-199">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="976f5-199">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="976f5-201">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="976f5-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="976f5-202">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="976f5-202">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="976f5-204">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="976f5-204">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="976f5-206">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="976f5-206">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="976f5-208">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="976f5-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="976f5-210">a.</span><span class="sxs-lookup"><span data-stu-id="976f5-210">a.</span></span> <span data-ttu-id="976f5-211">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="976f5-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="976f5-212">b.</span><span class="sxs-lookup"><span data-stu-id="976f5-212">b.</span></span> <span data-ttu-id="976f5-213">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="976f5-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="976f5-214">c.</span><span class="sxs-lookup"><span data-stu-id="976f5-214">c.</span></span> <span data-ttu-id="976f5-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="976f5-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="976f5-216">d.</span><span class="sxs-lookup"><span data-stu-id="976f5-216">d.</span></span> <span data-ttu-id="976f5-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="976f5-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="976f5-218">Vytvoření zkušebního uživatele Domo</span><span class="sxs-lookup"><span data-stu-id="976f5-218">Creating a Domo test user</span></span>

<span data-ttu-id="976f5-219">Hello cílem této části je toocreate volal Britta Simon v Domo uživatele.</span><span class="sxs-lookup"><span data-stu-id="976f5-219">hello objective of this section is toocreate a user called Britta Simon in Domo.</span></span> <span data-ttu-id="976f5-220">Domo podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="976f5-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="976f5-221">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="976f5-221">There is no action item for you in this section.</span></span> <span data-ttu-id="976f5-222">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Domo.</span><span class="sxs-lookup"><span data-stu-id="976f5-222">A new user is created during an attempt tooaccess Domo if it doesn't exist yet.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="976f5-223">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="976f5-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="976f5-224">V této části povolíte tak, že udělíte přístup tooDomo toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="976f5-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDomo.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="976f5-226">**tooassign Britta Simon tooDomo, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="976f5-226">**tooassign Britta Simon tooDomo, perform hello following steps:**</span></span>

1. <span data-ttu-id="976f5-227">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="976f5-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="976f5-229">V seznamu aplikace hello vyberte **Domo**.</span><span class="sxs-lookup"><span data-stu-id="976f5-229">In hello applications list, select **Domo**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="976f5-231">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="976f5-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="976f5-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="976f5-233">Click **Add** button.</span></span> <span data-ttu-id="976f5-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="976f5-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="976f5-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="976f5-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="976f5-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="976f5-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="976f5-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="976f5-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="976f5-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="976f5-239">Testing single sign-on</span></span>

<span data-ttu-id="976f5-240">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="976f5-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="976f5-241">Když kliknete na dlaždici Domo hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Domo aplikace.</span><span class="sxs-lookup"><span data-stu-id="976f5-241">When you click hello Domo tile in hello Access Panel, you should get automatically signed-on tooyour Domo application.</span></span>

<span data-ttu-id="976f5-242">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="976f5-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="976f5-243">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="976f5-243">Additional resources</span></span>

* [<span data-ttu-id="976f5-244">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="976f5-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="976f5-245">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="976f5-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

