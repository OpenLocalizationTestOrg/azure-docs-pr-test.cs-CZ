---
title: 'Kurz: Azure Active Directory integrace s DigiCert | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a DigiCert."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 646f3129-aa67-4875-9073-1d0b6a3173d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 861039d00533b3aeb361d04e45c4460c6fc8cef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-digicert"></a><span data-ttu-id="5027a-103">Kurz: Azure Active Directory integrace s DigiCert</span><span class="sxs-lookup"><span data-stu-id="5027a-103">Tutorial: Azure Active Directory integration with DigiCert</span></span>

<span data-ttu-id="5027a-104">V tomto kurzu zjistíte, jak toointegrate DigiCert s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5027a-104">In this tutorial, you learn how toointegrate DigiCert with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5027a-105">Integrace DigiCert s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5027a-105">Integrating DigiCert with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5027a-106">Můžete řídit ve službě Azure AD, který má přístup tooDigiCert</span><span class="sxs-lookup"><span data-stu-id="5027a-106">You can control in Azure AD who has access tooDigiCert</span></span>
- <span data-ttu-id="5027a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooDigiCert (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5027a-107">You can enable your users tooautomatically get signed-on tooDigiCert (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5027a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5027a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5027a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5027a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5027a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5027a-110">Prerequisites</span></span>

<span data-ttu-id="5027a-111">Integrace služby Azure AD s DigiCert tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5027a-111">tooconfigure Azure AD integration with DigiCert, you need hello following items:</span></span>

- <span data-ttu-id="5027a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5027a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5027a-113">DigiCert jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5027a-113">A DigiCert single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5027a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5027a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5027a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5027a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5027a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5027a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5027a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5027a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5027a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5027a-118">Scenario description</span></span>
<span data-ttu-id="5027a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5027a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5027a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5027a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5027a-121">Přidání DigiCert z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5027a-121">Adding DigiCert from hello gallery</span></span>
2. <span data-ttu-id="5027a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5027a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-digicert-from-hello-gallery"></a><span data-ttu-id="5027a-123">Přidání DigiCert z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="5027a-123">Adding DigiCert from hello gallery</span></span>
<span data-ttu-id="5027a-124">tooconfigure hello integrace DigiCert do Azure AD, je nutné tooadd DigiCert hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5027a-124">tooconfigure hello integration of DigiCert into Azure AD, you need tooadd DigiCert from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5027a-125">**tooadd DigiCert z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5027a-125">**tooadd DigiCert from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5027a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5027a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5027a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5027a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5027a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5027a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5027a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5027a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5027a-133">Hello vyhledávacího pole zadejte **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="5027a-133">In hello search box, type **DigiCert**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_search.png)

5. <span data-ttu-id="5027a-135">Na panelu výsledků hello vyberte **DigiCert**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5027a-135">In hello results panel, select **DigiCert**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5027a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5027a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5027a-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s DigiCert podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5027a-138">In this section, you configure and test Azure AD single sign-on with DigiCert based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5027a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v DigiCert je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5027a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DigiCert is tooa user in Azure AD.</span></span> <span data-ttu-id="5027a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v DigiCert musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="5027a-140">In other words, a link relationship between an Azure AD user and hello related user in DigiCert needs toobe established.</span></span>

<span data-ttu-id="5027a-141">V DigiCert, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="5027a-141">In DigiCert, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5027a-142">tooconfigure a testu Azure AD jednotné přihlašování s DigiCert, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5027a-142">tooconfigure and test Azure AD single sign-on with DigiCert, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5027a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="5027a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5027a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5027a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5027a-145">**[Vytvoření zkušebního uživatele DigiCert](#creating-a-digicert-test-user)**  -toohave protějšek Britta Simon v DigiCert, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="5027a-145">**[Creating a DigiCert test user](#creating-a-digicert-test-user)** - toohave a counterpart of Britta Simon in DigiCert that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5027a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5027a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5027a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="5027a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5027a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5027a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5027a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci DigiCert.</span><span class="sxs-lookup"><span data-stu-id="5027a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DigiCert application.</span></span>

<span data-ttu-id="5027a-150">**tooconfigure Azure AD jednotné přihlašování s DigiCert, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5027a-150">**tooconfigure Azure AD single sign-on with DigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="5027a-151">V portálu Azure, na hello hello **DigiCert** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5027a-151">In hello Azure portal, on hello **DigiCert** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5027a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5027a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_samlbase.png)

3. <span data-ttu-id="5027a-155">Na hello **DigiCert domény a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="5027a-155">On hello **DigiCert Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_url.png)

4. <span data-ttu-id="5027a-157">Aplikace DigiCert očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="5027a-157">DigiCert application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="5027a-158">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5027a-158">Configure hello following claims for this application.</span></span> <span data-ttu-id="5027a-159">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="5027a-159">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="5027a-160">Hello následující snímek obrazovky ukazuje příklad pro tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="5027a-160">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_attributes.png)
    
5. <span data-ttu-id="5027a-162">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5027a-162">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="5027a-163">Název atributu</span><span class="sxs-lookup"><span data-stu-id="5027a-163">Attribute Name</span></span> | <span data-ttu-id="5027a-164">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="5027a-164">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="5027a-165">Společnosti</span><span class="sxs-lookup"><span data-stu-id="5027a-165">company</span></span> | <span data-ttu-id="5027a-166">< companycode ></span><span class="sxs-lookup"><span data-stu-id="5027a-166">< companycode ></span></span> |
    | <span data-ttu-id="5027a-167">digicertrole</span><span class="sxs-lookup"><span data-stu-id="5027a-167">digicertrole</span></span> | <span data-ttu-id="5027a-168">CanAccessCertCentral</span><span class="sxs-lookup"><span data-stu-id="5027a-168">CanAccessCertCentral</span></span> |

    > [!Note]
    > <span data-ttu-id="5027a-169">Hello hodnotu **společnosti** atribut není skutečné.</span><span class="sxs-lookup"><span data-stu-id="5027a-169">hello value of **company** attribute is not real.</span></span> <span data-ttu-id="5027a-170">Aktualizujte tuto hodnotu s kódem skutečné společnosti.</span><span class="sxs-lookup"><span data-stu-id="5027a-170">Update this value with actual company code.</span></span> <span data-ttu-id="5027a-171">Hodnota hello tooget **společnosti** atribut kontaktujte [tým podpory DigiCert](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="5027a-171">tooget hello value of **company** attribute contact [DigiCert support team](mailto:support@digicert.com).</span></span>

    <span data-ttu-id="5027a-172">a.</span><span class="sxs-lookup"><span data-stu-id="5027a-172">a.</span></span> <span data-ttu-id="5027a-173">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5027a-173">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="5027a-176">b.</span><span class="sxs-lookup"><span data-stu-id="5027a-176">b.</span></span> <span data-ttu-id="5027a-177">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="5027a-177">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="5027a-178">c.</span><span class="sxs-lookup"><span data-stu-id="5027a-178">c.</span></span> <span data-ttu-id="5027a-179">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="5027a-179">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="5027a-180">d.</span><span class="sxs-lookup"><span data-stu-id="5027a-180">d.</span></span> <span data-ttu-id="5027a-181">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="5027a-181">Click **Ok**.</span></span> 

6. <span data-ttu-id="5027a-182">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="5027a-182">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_certificate.png) 

7. <span data-ttu-id="5027a-184">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5027a-184">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="5027a-186">tooconfigure jednotného přihlašování na **DigiCert** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[tým podpory DigiCert](mailto:support@digicert.com).</span><span class="sxs-lookup"><span data-stu-id="5027a-186">tooconfigure single sign-on on **DigiCert** side, you need toosend hello downloaded **Metadata XML** too[DigiCert support team](mailto:support@digicert.com).</span></span> <span data-ttu-id="5027a-187">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5027a-187">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5027a-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="5027a-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5027a-189">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="5027a-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5027a-190">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5027a-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5027a-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5027a-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="5027a-192">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="5027a-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5027a-194">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5027a-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5027a-195">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5027a-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5027a-197">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5027a-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5027a-199">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="5027a-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5027a-201">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5027a-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-digicert-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5027a-203">a.</span><span class="sxs-lookup"><span data-stu-id="5027a-203">a.</span></span> <span data-ttu-id="5027a-204">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5027a-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5027a-205">b.</span><span class="sxs-lookup"><span data-stu-id="5027a-205">b.</span></span> <span data-ttu-id="5027a-206">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5027a-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5027a-207">c.</span><span class="sxs-lookup"><span data-stu-id="5027a-207">c.</span></span> <span data-ttu-id="5027a-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5027a-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5027a-209">d.</span><span class="sxs-lookup"><span data-stu-id="5027a-209">d.</span></span> <span data-ttu-id="5027a-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5027a-210">Click **Create**.</span></span>
 
### <a name="creating-a-digicert-test-user"></a><span data-ttu-id="5027a-211">Vytvoření zkušebního uživatele DigiCert</span><span class="sxs-lookup"><span data-stu-id="5027a-211">Creating a DigiCert test user</span></span>

<span data-ttu-id="5027a-212">V této části vytvoříte volal Britta Simon v DigiCert uživatele.</span><span class="sxs-lookup"><span data-stu-id="5027a-212">In this section, you create a user called Britta Simon in DigiCert.</span></span> <span data-ttu-id="5027a-213">Spojte se s [tým podpory DigiCert](mailto:support@digicert.com) tooadd hello uživatele v DigiCert.</span><span class="sxs-lookup"><span data-stu-id="5027a-213">Please work with [DigiCert support team](mailto:support@digicert.com) tooadd hello users in DigiCert.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5027a-214">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="5027a-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5027a-215">V této části povolíte tak, že udělíte přístup tooDigiCert toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5027a-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDigiCert.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5027a-217">**tooassign Britta Simon tooDigiCert, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="5027a-217">**tooassign Britta Simon tooDigiCert, perform hello following steps:**</span></span>

1. <span data-ttu-id="5027a-218">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5027a-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5027a-220">V seznamu aplikace hello vyberte **DigiCert**.</span><span class="sxs-lookup"><span data-stu-id="5027a-220">In hello applications list, select **DigiCert**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-digicert-tutorial/tutorial_digicert_app.png) 

3. <span data-ttu-id="5027a-222">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5027a-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5027a-224">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5027a-224">Click **Add** button.</span></span> <span data-ttu-id="5027a-225">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5027a-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5027a-227">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="5027a-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5027a-228">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5027a-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5027a-229">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5027a-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5027a-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5027a-230">Testing single sign-on</span></span>

<span data-ttu-id="5027a-231">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5027a-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5027a-232">Když kliknete na dlaždici DigiCert hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour DeigiCert aplikace.</span><span class="sxs-lookup"><span data-stu-id="5027a-232">When you click hello DigiCert tile in hello Access Panel, you should get automatically signed-on tooyour DeigiCert application.</span></span>
<span data-ttu-id="5027a-233">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5027a-233">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5027a-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5027a-234">Additional resources</span></span>

* [<span data-ttu-id="5027a-235">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5027a-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5027a-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5027a-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-digicert-tutorial/tutorial_general_203.png

