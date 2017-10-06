---
title: 'Kurz: Azure Active Directory integrace s iLMS | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="cc711-103">Kurz: Azure Active Directory integrace s iLMS</span><span class="sxs-lookup"><span data-stu-id="cc711-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="cc711-104">V tomto kurzu zjistíte, jak toointegrate iLMS s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cc711-104">In this tutorial, you learn how toointegrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cc711-105">Integrace iLMS s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cc711-105">Integrating iLMS with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cc711-106">Můžete řídit ve službě Azure AD, který má přístup tooiLMS</span><span class="sxs-lookup"><span data-stu-id="cc711-106">You can control in Azure AD who has access tooiLMS</span></span>
- <span data-ttu-id="cc711-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooiLMS (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc711-107">You can enable your users tooautomatically get signed-on tooiLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cc711-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cc711-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cc711-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cc711-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc711-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc711-110">Prerequisites</span></span>

<span data-ttu-id="cc711-111">Integrace služby Azure AD s iLMS tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cc711-111">tooconfigure Azure AD integration with iLMS, you need hello following items:</span></span>

- <span data-ttu-id="cc711-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc711-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cc711-113">ILMS jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cc711-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cc711-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc711-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cc711-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cc711-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cc711-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="cc711-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cc711-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc711-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cc711-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cc711-118">Scenario description</span></span>
<span data-ttu-id="cc711-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc711-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cc711-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cc711-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cc711-121">Přidání iLMS z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cc711-121">Adding iLMS from hello gallery</span></span>
2. <span data-ttu-id="cc711-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc711-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-hello-gallery"></a><span data-ttu-id="cc711-123">Přidání iLMS z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cc711-123">Adding iLMS from hello gallery</span></span>
<span data-ttu-id="cc711-124">tooconfigure hello integrace iLMS do Azure AD, je nutné tooadd iLMS hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cc711-124">tooconfigure hello integration of iLMS into Azure AD, you need tooadd iLMS from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cc711-125">**tooadd iLMS z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cc711-125">**tooadd iLMS from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cc711-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cc711-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cc711-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cc711-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cc711-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cc711-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cc711-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cc711-131">tooadd new application, click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cc711-133">Hello vyhledávacího pole zadejte **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="cc711-133">In hello search box, type **iLMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="cc711-135">Na panelu výsledků hello vyberte **iLMS**, pak klikněte na tlačítko **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc711-135">In hello results panel, select **iLMS**, then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cc711-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc711-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cc711-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s iLMS podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cc711-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cc711-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v iLMS je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc711-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in iLMS is tooa user in Azure AD.</span></span> <span data-ttu-id="cc711-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v iLMS musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cc711-140">In other words, a link relationship between an Azure AD user and hello related user in iLMS needs toobe established.</span></span>

<span data-ttu-id="cc711-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v iLMS.</span><span class="sxs-lookup"><span data-stu-id="cc711-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in iLMS.</span></span>

<span data-ttu-id="cc711-142">tooconfigure a testu Azure AD jednotné přihlašování s iLMS, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cc711-142">tooconfigure and test Azure AD single sign-on with iLMS, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cc711-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cc711-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cc711-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc711-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cc711-145">**[Vytváření testovacího uživatele iLMS](#creating-an-ilms-test-user)**  -toohave protějšek Britta Simon v iLMS, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc711-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - toohave a counterpart of Britta Simon in iLMS that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="cc711-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc711-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cc711-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cc711-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cc711-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc711-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cc711-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci iLMS.</span><span class="sxs-lookup"><span data-stu-id="cc711-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="cc711-150">**tooconfigure Azure AD jednotné přihlašování s iLMS, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cc711-150">**tooconfigure Azure AD single sign-on with iLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="cc711-151">V portálu Azure, na hello hello **iLMS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cc711-151">In hello Azure portal, on hello **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cc711-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc711-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="cc711-155">Na hello **iLMS domény a adresy URL** část, proveďte následující kroky, pokud chcete aplikace hello tooconfigure hello **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="cc711-155">On hello **iLMS Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="cc711-157">a.</span><span class="sxs-lookup"><span data-stu-id="cc711-157">a.</span></span> <span data-ttu-id="cc711-158">V hello **identifikátor** textovému poli, vložte hello **identifikátor** hodnotu zkopírujete z **poskytovatele služeb** SAML nastaveních v portálu pro správu iLMS oddílu.</span><span class="sxs-lookup"><span data-stu-id="cc711-158">In hello **Identifier** textbox, paste hello **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="cc711-159">b.</span><span class="sxs-lookup"><span data-stu-id="cc711-159">b.</span></span> <span data-ttu-id="cc711-160">V hello **adresa URL odpovědi** textovému poli, vložte hello **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS s následující hello vzor`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="cc711-160">In hello **Reply URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having hello following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="cc711-161">Tato 123456 je příkladem hodnoty identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="cc711-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="cc711-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="cc711-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="cc711-164">V hello **přihlašovací adresa URL** textovému poli, vložte hello **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS jako`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="cc711-164">In hello **Sign-on URL** textbox, paste hello **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="cc711-165">tooenable JIT zřizování, iLMS aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="cc711-165">tooenable JIT provisioning, iLMS application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="cc711-166">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc711-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="cc711-167">Můžete spravovat hello hodnoty těchto atributů z hello **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc711-167">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="cc711-168">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="cc711-168">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="cc711-170">Vytvoření **oddělení, oblast** a **dělení** atributy a přidejte název hello těchto atributů v iLMS.</span><span class="sxs-lookup"><span data-stu-id="cc711-170">Create **Department, Region** and **Division** attributes and add hello name of these attributes in iLMS.</span></span> <span data-ttu-id="cc711-171">Všechny tyto atributy, které jsou uvedené výše se vyžadují.</span><span class="sxs-lookup"><span data-stu-id="cc711-171">All these attributes shown above are required.</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="cc711-172">Máte tooenable **vytvořit uživatelský účet Un-recognized** v iLMS toomap těchto atributů.</span><span class="sxs-lookup"><span data-stu-id="cc711-172">You have tooenable **Create Un-recognized User Account** in iLMS toomap these attributes.</span></span> <span data-ttu-id="cc711-173">Postupujte podle pokynů hello [sem](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget představu v konfiguraci atributy hello.</span><span class="sxs-lookup"><span data-stu-id="cc711-173">Follow hello instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget an idea on hello attributes configuration.</span></span>

6. <span data-ttu-id="cc711-174">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v hello obrázku výše a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc711-174">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="cc711-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="cc711-175">Attribute Name</span></span> | <span data-ttu-id="cc711-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="cc711-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="cc711-177">dělení</span><span class="sxs-lookup"><span data-stu-id="cc711-177">division</span></span> | <span data-ttu-id="cc711-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="cc711-178">user.department</span></span> |
    | <span data-ttu-id="cc711-179">Oblast</span><span class="sxs-lookup"><span data-stu-id="cc711-179">region</span></span> | <span data-ttu-id="cc711-180">User.state</span><span class="sxs-lookup"><span data-stu-id="cc711-180">user.state</span></span> |
    | <span data-ttu-id="cc711-181">Oddělení</span><span class="sxs-lookup"><span data-stu-id="cc711-181">department</span></span> | <span data-ttu-id="cc711-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="cc711-182">user.jobtitle</span></span> |

    <span data-ttu-id="cc711-183">a.</span><span class="sxs-lookup"><span data-stu-id="cc711-183">a.</span></span> <span data-ttu-id="cc711-184">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc711-184">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="cc711-187">b.</span><span class="sxs-lookup"><span data-stu-id="cc711-187">b.</span></span> <span data-ttu-id="cc711-188">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="cc711-188">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="cc711-189">c.</span><span class="sxs-lookup"><span data-stu-id="cc711-189">c.</span></span> <span data-ttu-id="cc711-190">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="cc711-190">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="cc711-191">d.</span><span class="sxs-lookup"><span data-stu-id="cc711-191">d.</span></span> <span data-ttu-id="cc711-192">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="cc711-192">Click **Ok**</span></span>

7. <span data-ttu-id="cc711-193">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cc711-193">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="cc711-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cc711-195">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="cc711-197">V okně prohlížeče jiných webových přihlásit tooyour **portál pro správu iLMS** jako správce.</span><span class="sxs-lookup"><span data-stu-id="cc711-197">In a different web browser window, log in tooyour **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="cc711-198">Klikněte na tlačítko **SSO:SAML** pod **nastavení** tooopen SAML nastavení a proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc711-198">Click **SSO:SAML** under **Settings** tab tooopen SAML settings and perform hello following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="cc711-200">a.</span><span class="sxs-lookup"><span data-stu-id="cc711-200">a.</span></span> <span data-ttu-id="cc711-201">Rozbalte hello **poskytovatele služeb** části a zkopírujte hello **identifikátor** a **koncový bod (URL)** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cc711-201">Expand hello **Service Provider** section and copy hello **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="cc711-203">b.</span><span class="sxs-lookup"><span data-stu-id="cc711-203">b.</span></span> <span data-ttu-id="cc711-204">V části **zprostředkovatele Identity** klikněte na tlačítko **importovat Metadata**.</span><span class="sxs-lookup"><span data-stu-id="cc711-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="cc711-205">c.</span><span class="sxs-lookup"><span data-stu-id="cc711-205">c.</span></span> <span data-ttu-id="cc711-206">Vyberte hello **Metadata** soubor stažený z portálu Azure z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="cc711-206">Select hello **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="cc711-208">d.</span><span class="sxs-lookup"><span data-stu-id="cc711-208">d.</span></span> <span data-ttu-id="cc711-209">Pokud chcete, aby tooenable JIT zřizování toocreate iLMS účty pro zrušení-rozpoznat uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="cc711-209">If you want tooenable JIT provisioning toocreate iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="cc711-210">Zkontrolujte **vytvořte účet bez rozpoznaný uživatel**.</span><span class="sxs-lookup"><span data-stu-id="cc711-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="cc711-212">Mapování atributů hello ve službě Azure AD s atributy hello v iLMS.</span><span class="sxs-lookup"><span data-stu-id="cc711-212">Map hello attributes in Azure AD with hello attributes in iLMS.</span></span> <span data-ttu-id="cc711-213">Ve sloupci atributu hello zadejte hello atributy názvu nebo hello výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cc711-213">In hello attribute column, specify hello attributes name or hello default value.</span></span>

    <span data-ttu-id="cc711-214">e.</span><span class="sxs-lookup"><span data-stu-id="cc711-214">e.</span></span> <span data-ttu-id="cc711-215">Přejděte příliš**obchodní pravidla** kartě a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc711-215">Go too**Business Rules** tab and perform hello following steps:</span></span> 
        
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="cc711-217">Zkontrolujte **vytvořit Un-recognized oblastí, rozdělení a oddělení** toocreate oblastí divizí a oddělení, které už neexistují v hello době jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc711-217">Check **Create Un-recognized Regions, Divisions and Departments** toocreate Regions, Divisions, and Departments that do not already exist at hello time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="cc711-218">Zkontrolujte **aktualizace uživatelského profilu při přihlášení** toospecify jestli aktualizaci profilu hello uživatele s každou jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc711-218">Check **Update User Profile During Sign-in** toospecify whether hello user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="cc711-219">Pokud hello **"Aktualizace prázdné hodnoty pro jiný povinného pole v profilu uživatele"** zaškrtnutá možnost, volitelné profil pole, která jsou prázdné po přihlášení bude také způsobit profil iLMS hello uživatele toocontain prázdné hodnoty těchto polí.</span><span class="sxs-lookup"><span data-stu-id="cc711-219">If hello **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause hello user’s iLMS profile toocontain blank values for those fields.</span></span>
        
       - <span data-ttu-id="cc711-220">Zkontrolujte **odeslat E-mail s oznámením chyba** a zadejte e-mailu hello hello uživatele, které chcete tooreceive hello chyba oznámení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="cc711-220">Check **Send Error Notification Email** and enter hello email of hello user where you want tooreceive hello error notification email.</span></span>

11. <span data-ttu-id="cc711-221">Klikněte na tlačítko **Uložit** tlačítko toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="cc711-221">Click **Save** button toosave hello settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="cc711-223">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cc711-223">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cc711-224">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cc711-224">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cc711-225">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cc711-225">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cc711-226">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc711-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="cc711-227">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cc711-227">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cc711-229">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cc711-229">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cc711-230">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cc711-230">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cc711-232">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cc711-232">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cc711-234">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc711-234">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cc711-236">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc711-236">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cc711-238">a.</span><span class="sxs-lookup"><span data-stu-id="cc711-238">a.</span></span> <span data-ttu-id="cc711-239">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cc711-239">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cc711-240">b.</span><span class="sxs-lookup"><span data-stu-id="cc711-240">b.</span></span> <span data-ttu-id="cc711-241">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cc711-241">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cc711-242">c.</span><span class="sxs-lookup"><span data-stu-id="cc711-242">c.</span></span> <span data-ttu-id="cc711-243">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cc711-243">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cc711-244">d.</span><span class="sxs-lookup"><span data-stu-id="cc711-244">d.</span></span> <span data-ttu-id="cc711-245">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cc711-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="cc711-246">Vytváření testovacího uživatele iLMS</span><span class="sxs-lookup"><span data-stu-id="cc711-246">Creating an iLMS test user</span></span>

<span data-ttu-id="cc711-247">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele v aplikaci hello automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="cc711-247">Application supports Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="cc711-248">JIT bude fungovat, pokud jste klikli hello **vytvořit uživatelský účet Un-recognized** políčko během SAML nastavení konfigurace na portál pro správu iLMS.</span><span class="sxs-lookup"><span data-stu-id="cc711-248">JIT will work, if you have clicked hello **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="cc711-249">Pokud potřebujete toocreate uživatelé ručně, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="cc711-249">If you need toocreate an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="cc711-250">Přihlaste se na webu společnosti iLMS tooyour jako správce.</span><span class="sxs-lookup"><span data-stu-id="cc711-250">Log in tooyour iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="cc711-251">Klikněte na tlačítko **"Uživatel zaregistrovat"** pod **uživatelé** kartě tooopen **registrovat uživatele** stránky.</span><span class="sxs-lookup"><span data-stu-id="cc711-251">Click **“Register User”** under **Users** tab tooopen **Register User** page.</span></span> 
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="cc711-253">Na hello **"Uživatel zaregistrovat"** proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="cc711-253">On hello **“Register User”** page, perform hello following steps.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="cc711-255">a.</span><span class="sxs-lookup"><span data-stu-id="cc711-255">a.</span></span> <span data-ttu-id="cc711-256">V hello **křestní jméno** textovému poli, typ hello křestní jméno Britta.</span><span class="sxs-lookup"><span data-stu-id="cc711-256">In hello **First Name** textbox, type hello first name Britta.</span></span>
   
    <span data-ttu-id="cc711-257">b.</span><span class="sxs-lookup"><span data-stu-id="cc711-257">b.</span></span> <span data-ttu-id="cc711-258">V hello **příjmení** textovému poli, typ hello příjmení Simon.</span><span class="sxs-lookup"><span data-stu-id="cc711-258">In hello **Last Name** textbox, type hello last name Simon.</span></span>

    <span data-ttu-id="cc711-259">c.</span><span class="sxs-lookup"><span data-stu-id="cc711-259">c.</span></span> <span data-ttu-id="cc711-260">V hello **ID e-mailu** textovému poli, typ hello e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc711-260">In hello **Email ID** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="cc711-261">d.</span><span class="sxs-lookup"><span data-stu-id="cc711-261">d.</span></span> <span data-ttu-id="cc711-262">V hello **oblast** rozevíracího seznamu, vyberte hello hodnotu pro oblast.</span><span class="sxs-lookup"><span data-stu-id="cc711-262">In hello **Region** dropdown, select hello value for region.</span></span>

    <span data-ttu-id="cc711-263">e.</span><span class="sxs-lookup"><span data-stu-id="cc711-263">e.</span></span> <span data-ttu-id="cc711-264">V hello **dělení** rozevíracího seznamu, vyberte hello hodnotu pro dělení.</span><span class="sxs-lookup"><span data-stu-id="cc711-264">In hello **Division** dropdown, select hello value for division.</span></span>

    <span data-ttu-id="cc711-265">f.</span><span class="sxs-lookup"><span data-stu-id="cc711-265">f.</span></span> <span data-ttu-id="cc711-266">V hello **oddělení** rozevíracího seznamu, vyberte hello hodnotu pro oddělení.</span><span class="sxs-lookup"><span data-stu-id="cc711-266">In hello **Department** dropdown, select hello value for department.</span></span>

    <span data-ttu-id="cc711-267">g.</span><span class="sxs-lookup"><span data-stu-id="cc711-267">g.</span></span> <span data-ttu-id="cc711-268">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cc711-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cc711-269">Můžete odeslat e-mailu toouser registrace výběrem **odesílání pošty registrace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="cc711-269">You can send registration mail toouser by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cc711-270">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cc711-270">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cc711-271">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooiLMS svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="cc711-271">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooiLMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cc711-273">**tooassign Britta Simon tooiLMS, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cc711-273">**tooassign Britta Simon tooiLMS, perform hello following steps:**</span></span>

1. <span data-ttu-id="cc711-274">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cc711-274">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cc711-276">V seznamu aplikace hello vyberte **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="cc711-276">In hello applications list, select **iLMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="cc711-278">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cc711-278">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cc711-280">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cc711-280">Click **Add** button.</span></span> <span data-ttu-id="cc711-281">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc711-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cc711-283">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cc711-283">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cc711-284">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc711-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cc711-285">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc711-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cc711-286">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc711-286">Testing single sign-on</span></span>

<span data-ttu-id="cc711-287">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cc711-287">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cc711-288">Po kliknutí na tlačítko hello iLMS dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour iLMS aplikace.</span><span class="sxs-lookup"><span data-stu-id="cc711-288">When you click hello iLMS tile in hello Access Panel, you should get automatically signed-on tooyour iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc711-289">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cc711-289">Additional resources</span></span>

* [<span data-ttu-id="cc711-290">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc711-290">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cc711-291">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cc711-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

